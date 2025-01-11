### Runnable interface:

The Runnable interface should be implemented by any class whose instances are intended to be executed by a thread. Runnable interface have only one method named run().

1. **public void run():** is used to perform action for a thread.

```run-java
class Multi3 implements Runnable{  
	public void run(){  
		System.out.println("thread is running...");  
	}  
  
	public static void main(String args[]){  
		Multi3 m1=new Multi3();  
		Thread t1 =new Thread(m1);   // Using the constructor Thread(Runnable r)  
		t1.start();
	}  
}  
```
