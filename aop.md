```java
@Aspect
@Component
public class AspectJTest {

    @AfterReturning(value = "within(com.xlingmao.epoch.account.api.*)",returning = "retVal")
    public void cutPoint(JoinPoint joinPoint,Object retVal){

        System.out.println(joinPoint + "" + retVal);
        try {
            //return joinPoint;
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
        //return null;
    }

    @Around("execution (* com.xlingmao.epoch.account.api.*.*(..))")
    public Object cutPoint(ProceedingJoinPoint joinPoint){
        System.out.println("====>" + joinPoint.getThis());
        return joinPoint.getThis();
    }
}