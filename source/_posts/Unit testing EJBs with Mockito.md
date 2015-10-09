title: Unit testing EJBs with Mockito
date: 2011-02-20 12:49:00
tags: [EJB,JavaEE,mockito,unit test]
---

There's a lot of hype over in-container EJB testing frameworks, like [Arquillian](http://www.jboss.org/arquillian), that JBoss is trying to push. However, as even Arquillian's front page will tell you, this is integration testing, not unit testing. For actual unit tests, a module should be detached from everything else, especially the container.  

Fortunately, thanks to EJBs being just plain Java objects with annotations, they can be unit tested just like any other object, by mocking the dependencies.  

I created a small demo application to show how to test EJBs with [Mockito](http://mockito.org/), a mocking framework that makes mocking things like an EntityManager much easier. The application has a servlet, that when invoked, records the visit time to the database and returns a list of all visits in the HTTP response. Recording and reading the list of visits is handled by this EJB:  

<pre>@Stateless  
public class VisitsBean {  

 @PersistenceContext  
 EntityManager entityManager;  

 public void recordVisit() {  
  Visit visit = new Visit();  
  visit.setVisitTime(new Date());  
  entityManager.persist(visit);  
 }  

 @SuppressWarnings("unchecked")  
 public List <visit>getVisits() {  
  return entityManager.createNamedQuery(Visit.ALL).getResultList();  
 }  
}</visit></pre>

To test this EJB, we obviously need to create an EntityManager and test to see if correct methods are called on it, with correct parameters. This is how I did it with JUnit4 and Mockito:  

<pre>public class VisitsBeanTest {  

 private VisitsBean visitsBean = new VisitsBean();  

 @Before  
 public void injectMockEntityManager() {  
  EntityManager entityManager = mock(EntityManager.class);  
  visitsBean.entityManager = entityManager;  
 }  

 @Test  
 public void testRecordVisit() {  
  visitsBean.recordVisit();  

  ArgumentCaptor <visit>persistedVisit = ArgumentCaptor.forClass(Visit.class);  
  verify(visitsBean.entityManager, atLeastOnce())  
  .persist(persistedVisit.capture());  

  long visitTimeInMillis = persistedVisit.getValue().getVisitTime().getTime();  
  assertTrue("Visit time not set correctly",   
    System.currentTimeMillis() - visitTimeInMillis < TimeUnit.SECONDS.toMillis(1));  
 }  

 @Test  
 public void testGetVisits() {  
  List <visit>visitList = VisitRecorderTestUtil.createTestVisitList();  
  Query mockQuery = getQueryThatReturnsList(visitList);  
  when(visitsBean.entityManager.createNamedQuery(anyString()))  
  .thenReturn(mockQuery);  
  assertEquals(visitList, visitsBean.getVisits());  
  verify(visitsBean.entityManager).createNamedQuery(eq(Visit.ALL));  
 }  

 private Query getQueryThatReturnsList(List list) {  
  Query mockQuery = mock(Query.class);  
  when(mockQuery.getResultList()).thenReturn(list);  
  return mockQuery;  
 }  
}</visit></visit></pre>

Before every test, we call the _injectMockEntityManager()_ method, that uses Mockito to create a mock EntityManager and injects it into the EJB.  

The _testRecordVisit()_ method checks that _perisit()_ is called on the EntityManager, with correct time set for the Visit, and _testGetVisits()_ will check that correct named query is invoked to retrieve the list of Visits from the database and that results are returned.  

As we can see here, we achieved 100% test coverage, according to EclEmma:  

<div class="separator" style="clear: both; text-align: center;">[![](http://1.bp.blogspot.com/-w2e7qhjYyHI/TWEL_sVvTRI/AAAAAAAAABA/I7oyTEz2Qd8/s320/testcoverage.png)](http://1.bp.blogspot.com/-w2e7qhjYyHI/TWEL_sVvTRI/AAAAAAAAABA/I7oyTEz2Qd8/s1600/testcoverage.png)</div>

However, there are some problems with this approach:  

*   test are pretty brittle, if we would, for example, change the _getVisits()_ method to use Criteria queries, instead of invoking the named query,  the test would break
*   we are not testing things like transaction handling, but it could be argued that this is the container's responsebillity, so we're not supposed to test them
*   you really can't test entity beans this way, without a JPA provider, you can only test getters, setters and hashCode - equals contract

With that said, I do think that this is a good technique for crating fast and autonomous EJB unit tests. I'll try to use it in a more _real world_ project and report how it turns out.  

You can check out the full code for the demo project on my GitHub:  
[https://github.com/rkovacevic/Visit-recorder](https://github.com/rkovacevic/Visit-recorder)  
(I tested it on JBoss AS 6.0, but you can probably deploy it to Glassfish too, without much problems)