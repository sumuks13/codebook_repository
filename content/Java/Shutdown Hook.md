
tags: [[Thread]]

- A **shutdown hook** in Java is like a **last-minute cleanup crew** for your application.
- Imagine your program as a house party: when the party ends (your application shuts down), the shutdown hook ensures that everything is tidied up before everyone leaves.

```run-java
public class TestShutdown1{    
	public static void main(String[] args)throws Exception {    
    
		Runtime r=Runtime.getRuntime();    
		r.addShutdownHook(new MyThread());            
		
		try{
			System.out.println("Now main sleeping...");
			Thread.sleep(5000);
			System.exit(0);
		}
		catch (Exception e) {}    
	}    
}

class MyThread extends Thread{    
    public void run(){    
        System.out.println("shut down hook task completed..");    
    }    
}     
```

JVM Shutdowns when:

- user presses ctrl+c on the command prompt
- System.exit(int) method is invoked
- user logoff
- user shutdown etc.