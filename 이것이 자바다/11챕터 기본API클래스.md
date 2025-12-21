<h1>자바 API 도큐먼트</h1>

​	API 라이브러리는 프로그램 개발에 자주 사용되는 클래스 및 인터페이스의 모음이다.

​	<a>
​    http://docs.oracle.com/javase/8/docs/api/
</a>

<h3>java.lang과 java.util 패키지</h3>

​	공통적으로 가장 많이 사용하는 패키지는 java.lang 패키지와 java.util, java.time 패키일 것이다.

<h4>java.lang</h4>

​	기본적인 클래스를 담고 있는 패키지이다.

​	java.lang 패키지에 있는 클래스와 인터페이스는 import 하지 않아도 사용할 수 있다.

<table>
    <tr>
        <th>클래스</th>
        <th>용도</th>
    </tr>
    <tr>
    	<td>Object</td>
        <td>자바 클래스의 최상위 클래스</td>
    </tr>
    <tr>
    	<td>System</td>
        <td>표준입출력장치 제어 및 쓰레기 수집 및 자바 가상 머신 종료</td>
    </tr>
    <tr>
    	<td>Class</td>
        <td>클래스를 메모리로 로딩시, 사용</td>
    </tr>
    <tr>
    	<td>String</td>
        <td>문자열을 저장하고자 사용</td>
    </tr>
    <tr>
    	<td>StringBuffer, StringBuilder</td>
        <td>문자열을 삽입, 삭제 등과 같은 연산을 하기 위해 사용</td>
    </tr>
    <tr>
    	<td>Math</td>
        <td>수학적 계산을 사용하기 위해 사용</td>
    </tr>
    <tr>
        <td>Byte, Short, Character,Integer, Float, Double, Boolean, Long</td>
        <td>기본 타입의 데이터를 갖는 객체를 만들 때 사용</td>
    </tr>
</table>

<h3>java.util 패키지</h3>

​	java.util 패키지는 자바 프로그램 개발에 조미료 같은 역활을 하는 클래스를 담고 있다.

​	컬렉션 패키지들이 대부분이다.

<table>
    <tr>
        <th>클래스</th>
        <th>용도</th>
    </tr>
    <tr>
    	<td>Arrays</td>
        <td>배열을 조작(비교, 복사, 정렬, 찾기)할 때 사용</td>
    </tr>
    <tr>
    	<td>Calendar</td>
        <td>운영체제 날짜 및 시간을 얻을 때 사용</td>
    </tr>
    <tr>
    	<td>Date</td>
        <td>날짜와 시간 정보를 저장하는 클래스</td>
    </tr>
    <tr>
    	<td>Objects</td>
        <td>객체 비교, 널(null) 여부등을 조사할 때 사용</td>
    </tr>
    <tr>
    	<td>StringTokenizer</td>
        <td>특정 문자로 구분된 문자열을 구분할 때 사용</td>
    </tr>
    <tr>
    	<td>Random</td>
        <td>난수를 생성할 때 사용</td>
    </tr>
</table>

<h1>Object 클래스</h1>

​	자바의 모든 클래스는 java.lang.Object의 자식이다.

​	최상위 부모 클래스이다.

<h3>객체 비교(equals())</h3>

~~~java
public class Member{
    private String id;
    public Member(String id){
        this.id = id;
    }
    
    @Override
    public boolean equals(Object obj){
        if(obj instanceof Member){
            Member member = (Member)obj;
            if(id.equals(member.getId()))
                return true;
        }
        return false;
    }
    public String getId(){
        return id;
    }
}
public class MainClass{
	public static void main(String[] args){
        Member member1 = new Member("1");
        Member member2 = new Member("1");
        Member member3 = new Member("2");
        if(member1.equals(member2))
            System.out.println("member1와 member2는 동등하다.");
        else
            System.out.println("member1와 member2는 동등하지 않다.");
        
        if(member2.equals(member3))
            System.out.println("member2와 member3는 동등하다.");
        else
            System.out.println("member2와 member3는 동등하지 않다.");
	}
}
~~~



<h3>객체 해시코드(hashcode)</h3>

​	객체를 식별하는 하나의 정수값이다.

​	객체의 메모리 번지를 이용하여 해시코드를 만들어 리턴한다.

![hashcodeImage](/uploads/32ec421c0d25b5758897edbb98c2c01c/hashcodeImage.png)

~~~java
public class Key{
    private int key;
   	public Key(int key){
        this.key = key;
    }
    
    @Override
    public boolean equals(Object obj){
        if(obj instanceof Key){
            Key key = (Key)obj;
            if(this.key == key.getKey())
                return true;
        }
        return false;
    }
    
    @Override
    public int hashCode(){
        return this.key;
    }
    
    public int getKey(){
        return key;
    }
}

public class HashMapClass{
    public static void main(String[] args){
 		HashMap<Key,String> map = new HashMap<Key,String>();       
    	map.put(new Key(1),"홍길동");
        
        String value = map.get(new Key(1));
        System.out.println(value);
    }
}
~~~

객체의 동등 비교를 위해서는 equals만 재정의하지 말고 hashCode도 재정의해서 논리적 동등 객체일 경우 동일한 해시코드가 리턴되도록해야한다. 



<h2>객체 문자 정보(toString())</h2>

​	기본적으로 "클래스명@16진수해시코드"로 구성된 문자열을 리턴한다.

~~~java
public class MainClass{
    public static void main(String[] args){
        Object obj = new Object();
        System.out.println(obj.toString());
    }
}

public class SmartPhone{
    private String company;
    private String os;
    
    public SmartPhone(String company, String os){
        this.company = company;
        this.os = os;
    }
    
    @Override
    public String toString(){
        return company+", "+os;
    }
}

public class SmartPhoneEx{
    public static void main(String[] args){
        SmartPhone myPhone = new SmartPhone("구글","안드로이드");
        String str = myPhone.toString();
        System.out.println(str);
    }
}
~~~



<h2>객체 복제(clone)</h2>

1. 얕은 복제
2. 깊은 복제

<h4>얕은 복제(thin clone)</h4>

​	단순히 필드값만 복사하는 것

​	필드가 참조형일 경우, 객체 주소를 복사한다.

​	복사되는 객체는 Cloneable 인터페이스를 구현 받아야한다.[명시적 표시]

​	참조 객체를 복사한 경우, 한 쪽에서 변경시, 다른쪽도 변경됨 String예외

~~~java
public class Member implements Cloneable{
    private String id;
    private String name;
    private String password;
    private int age;
    private int[] scores;

    public Member(String id, String name, String password, int age, int[] scores){
        this.id = id;
        this.name = name;
        this.password = password;
        this.age = age;
        this.scores = scores;
    }

    public Member getMember(){
        Member cloned = null;
        try{
            cloned = (Member) clone();
        }catch(CloneNotSupportedException e){
            return null;
        }
        return cloned;
    }

    @Override
    public String toString(){
        return "Member{"
                +"ID: "+this.id
                +" name: "+this.name
                +" password: "+this.password
                +" age: "+this.age
                +" scores"+ Arrays.toString(scores)
                +"}";

    }

    public int[] getScores() {
        return scores;
    }

    public void setName(String name) {
        this.name = name;
    }
}

public class MainClass{
    public static void main(String[] args){
        Member member = new Member("1","홍길동","blue",30,new int[]{30,40,50});
        Member clonedMember = member.getMember();
        clonedMember.getScores()[0] = 100;
        clonedMember.setName("김자바");

        System.out.println(member);
        System.out.println(clonedMember);
    }
}
~~~

<h4>깊은 복제(deep clone)</h4>

​	깊은 복제는 참조하고 있는 객체도 복제한다.

~~~java
public class Member implements Cloneable{
    private String name;
    private int age;
    private int[] scores;
    private Car car;
    
    public Member(String name, int age, int[] scores, Car car){
        this.name = name;
        this.age = age;
        this.scores = scores;
        this.car = car;
    }
    
    @Override
    protected Object clone() throws CloneNotSupportedException{
        //얕은 복제
        Member cloned = (Member) super.clone();
        //깊은 복제
        cloned.scores = Arrays.copyOf(this.scores,this.scores.length);
        cloned.car = new Car(this.car.getModel());
        return cloned;
    }
    
    public Member getMember(){
        Member cloned = null;
        try{
            cloned = (Member) clone();
        }catch(CloneNotSupportedException e){}
        return cloned;
    }
    
    @Override
    public String toString(){
        return "Member{"
            +"name: "+this.name
            +" age: "+this.age
            +" scores: "+Arrays.toString(scores)
            +" car: "+car.getModel()
            +"}";
    }
    
    public int[] getScores(){
        return scores;
    }
    public Car getCar(){
        return car;
    }
    
}

public class Car{
    private String model;

    public Car(String model){
        this.model = model;
    }

    public String getModel() {
        return model;
    }
    public void setModel(String model){
        this.model = model;
    }
}

public class MainClass{
    public static void main(String[] args){
        Member member = new Member("김자바",30,new int[]{30,50,100},new Car("그랜저"));
        Member clonedMember = member.getMember();
       	clonedMember.getCar().setModel("소나타");
        clonedMember.getScores()[0] = 100;
        
        System.out.println(member);
        System.out.println(clonedMember);
    }
}

~~~

<h3>객체 소멸자(finalize())[자바 11버전 사용X권장]</h3>

​	자바는 참조하지 않는 메모리는 쓰레기 수집기가 자동적으로 소멸시킨다.

​	소멸 시키기 전에 finalize() 메소드를 실행시킨다.

~~~java
public class Counter{
    private int no;
    public Counter(int no){
        this.no = no;
    }
    
    @Override
    protected void finalize() throws Throwable{
        System.out.println(no+"번 객체 소멸");
    }
}

public class FinalizeClass{
    public static void main(String[] args){
        Counter counter = null;
        for(int i=1; i<=50; i++){
            counter = new Counter(i);
            
            counter = null;
            //쓰레기 수집기 실행 요청
            System.gc();
        }
        
    }
}
~~~

<h2>Objects 클래스</h2>

​	java.util.Objects는 객체비교, 해시코드 생성, null여부 체크 등을 하는 정적메소드를 가지고 있다.

<h4>객체비교(compare(T a, T b, Comparetor&lt;T&gt;c))</h4>

​	두 객체를 비교해서 int값으로 리턴한다.

~~~java
public class Student{
    private int sno;
    public Student(int sno){
        this.sno = sno;
    }
    
    public int getSno(){
        return sno;
    }
    
    public void setSno(int sno){
        this.sno = sno;
    }
}

public class StudentComparetor implements Comparetor<Student>{
    @Override
    public int compare(Student a, Student b){
        if(a.getSno() < b.getSno()) return -1;
        else if(a.getSno() == b.getSno()) return 0;
        else return 1;
    }
}

public class CompareClass{
    public static void main(String[] args){
        Student s1 = new Student(1);
        Student s2 = new Student(1);
        Student s3 = new Student(2);
        
        int result1 = Objects.compare(s1,s2, new StudentComparetor());
        int result2 = Objects.compare(s2,s3, new StudentComparetor());
        
    }
}

~~~

<h4>동등비교(Objects.equals() or Objects.deepEquals())</h4>

​	두 객체를 동등비교한다.

~~~java
public class ObjectsClass{
    public static void main(String[] args){
        Integer o1 = 1000;
        Integer o2 = 1000;
        System.out.println(Objects.equals(o1,o2));	//true
        System.out.println(Objects.equals(o1,null)); //false
        System.out.println(Objects.equals(null,o2)); //false
        System.out.println(Objects.equals(null,null)); //true
        System.out.println(Objects.deepEquals(o1,o2)); //true
		
        Integer[] arr1 = {1,2};
        Integer[] arr2 = {1,2};
        System.out.println(Objects.equals(arr1,arr2)); //false
        System.out.println(Objects.deepEquals(arr1,arr2)); //true
        System.out.println(Arrays.deepEquals(arr1,arr2)); //true
        System.out.println(Objects.deepEquals(null,arr2)); //false
        System.out.println(Arrays.deepEquals(arr1,null)); //false
        System.out.println(Arrays.deepEquals(null,null)); //true
    }
}
~~~

<h3>해시코드 생성(hash(), hashCode())</h3>

~~~java
public class Student{
    private int sno;
    private String name;
    public Student(int sno, String name){
        this.sno = sno;
        this.name = name;
    }
    @Override
    public int hashCode(){
        return Objects.hash(sno,name);
    }
}
public class HashCodeClass{
    public static void main(String[] args){
        Student s1 = new Student(1,"홍길동");
        Student s1 = new Student(1,"홍길동");
        //같은 해쉬코드
        System.out.println(s1.hashCode());	
        System.out.println(Objects.hash(s2));
    }
}
~~~

<h3>널 여부조사(isNull(), nonNull(), requireNonNull())</h3>

~~~java
public class NullClass{
    public static void main(String[] args){
        String str1 = "홍길동";
        String str2 = null;
        
        System.out.println(Objects.requireNonNull(str1));
        
        try{
            String name = Objects.requireNonNull(str2);
        }catch(Exception e){
            System.out.println(e.getMessage());
        }
        
        try{
            String name = Objects.requireNonNull(str2,"이름이 없습니다.");
        }catch(Exception e){
            System.out.println(e.getMessage());
        }
         try{
            String name = Objects.requireNonNull(str2,()->"이름이 없어요.");
        }catch(Exception e){
            System.out.println(e.getMessage());
        }
    }
}
~~~

<h3>객체 문자 정보(toString())</h3>

~~~java
public class ToStringClass{
    public static void main(String[] args){
        String str1 = "홍길동";
        String str2 = null;
        
        System.out.println(Objects.toString(str1));
        System.out.println(Objects.toString(str2));
        System.out.println(Objects.toString(str2,"이름이 없습니다."));
    }
}
~~~

<h1>System 클래스</h1>

​	System 클래스를 이용하면 운영체제의 일부 기능을 이용할 수 있다.

<h3>프로그램 종료(exit())</h3>

​	강제적으로 JVM을 종료시킬 때 사용한다.

​	정상 종료 : 0

​	비정상 종료 : 0 이외의 값

~~~java
public class ExitClass{
    public static void main(String[] args){
        //보안 관리자 설정
        System.setSecurityManger(new SecurityManger(){
           @Override
            public void checkExit(int status){
                if(status != 5)
                    throw new SecurityException();
            }
        });
        
        for(int i=0; i<10; i++){
            System.out.println(i);
            try{
                System.exit(i);
            }catch(SecurityException e){
                
            }
        }
    }
}
~~~

<h3>쓰레기 수집기 실행(gc())</h3>

​	메모리는 JVM이 알아서 자동으로 관리한다.

​	JVM은 메모리가 부족시, CPU 유휴시간을 이용해 쓰레기 수집가(Garbage Collector)를 실행시켜 사용하지 않는 객체를 자동으로 제거한다.

​	System.gc() 메소드를 사용하면 쓰레기를 가능한 빨리 수집해달라고 요청한다.

~~~java
public class Employee{
    private int eno;
    public Employee(int eno){
        this.eno = eno;
        System.out.println("Employee("+eno+") 가 메모리에서 생성됨");
    }
    @Override
   	public void finalize(){
        System.out.println("Employee("+eno+") 이 메모리에서 재거됨");
    }
    public int getEno(){
        return eno;
    }
}
public class GcClass{
    public static void main(String[] args){
        Employee emp;
        emp = new Employee(1);
        emp = null;
        emp = new Employee(2);
        emp = new Employee(3);
        System.out.println("emp가 최종적으로 참조하는 시원번호: ");
        System.out.println(emp.getEno());
        System.gc();
    }
}
~~~

<h3>현재 시각 읽기(currentTimeMillis(), nanoTime())</h3>

~~~java
public class SystemTimeClass{
    public static void main(String[] args){
        long time1 = System.nanoTime();
        
        int sum = 0;
        for(int i=1; i<=100000; i++)
            sum += i;
        
        long time2 = System.nanoTime();
        
        System.out.println("1~100000까지의 합: "+sum);
        System.out.println("계산에 "+(time2-time1)+" 나노초가 소요되었습니다.");
    }
}
~~~

<h3>현재 프로퍼티 읽기(getProperty())</h3>

​	시스템의 속성값을 알 수 있다.

~~~java
public class GetPropertyClass{
    public static void main(String[] args){
        String osName = System.getProperty("os.name");
        String userName = System.getProperty("user.name");
        String userHome = System.getProperty("user.home");
        
        System.out.println("운영체제 이름: "+osName);
        System.out.println("사용자 이름: "+userName);
        System.out.println("사용자 홈디렉토리: "+userHome);
        
        System.out.println("--------------------------");
        System.out.println(" [ key ]  value");
        System.out.println("--------------------------");
        
        Properties props = System.getProperties();
        Set keys = props.keySet();
        for(Object objKey : keys){
            String key = (String) objKey;
            String value = System.getProperty(key);
            System.out.println(" [ key ]  "+value);
        }
    }
}
~~~

<h3>환경 변수 읽기(getenv())</h3>

~~~java
public class SystemClass{
    public static void main(String[] args){
        String javaHome = System.getenv("JAVA_HOME");
        System.out.println("JAVA_HOME: "+javaHome);
    }
}
~~~

<h1>Class 클래스</h1>

​	메타데이터 정보를 가지고 있다.

​	메타데이터란 클래스의 이름, 생성자정보, 필드 정보, 메소드 정보이다.

~~~~java
public class Car(){
    String name;
    public Car(){}
    public void method(){}
}

public class ClassEx{
    public static void main(String[] args){
        Car car = new Car();
        Class clazz1 = car.getClass();
        System.out.println(clazz1.getName());
        System.out.println(clazz1.getSimpleName());
        System.out.println(clazz1.getPackage().getName());
        System.out.println();
        
        try{
            Class clazz2 = Class.forName("Car");
            System.out.println(clazz2.getName());
       	 	System.out.println(clazz2.getSimpleName());
        	System.out.println(clazz2.getPackage().getName());
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }
    }
}
~~~~

<h2>리플렉션</h2>

​	java.lang.reflect 패키지에 속해있다.

​	필드 배열, 메소드 배열, 생성자 배열을 리턴한다.

~~~java
public class Car(){
    String name;
    public Car(){}
    public void method(){}
}
public class RelectionClass{
    public static void main(String[] args){
        Class clazz = Class.forName("Car");
        
        System.out.println("[클래스 이름]");
        System.out.println(clazz.getName());
        System.out.println();
        
        System.out.println("[생성자 정보]");
        Constructor[] constructors = clazz.getDeclearedConstructor();
        for(Constructor cons : constructors){
            System.out.print(cons.getName()+"(");
            Class[] parameters = cons.getParameterTypes();
            printParameters(parameters);
            System.out.println(")");
        }
        System.out.println();
        
        System.out.println("[필드 정보]");
        Field[] fields = clazz.getDeclearedFiedls();
        for(Field field : fields){
            System.out.println(field.getType().getSimpleName()+" "+field.getName());
        }
       	System.out.println();
        
        System.out.println("[메소드 정보]");
        Method[] methods = clazz.getDeclearedMethods();
        for(Method method : methods){
            System.out.print(method.getName() +"(");
            Class[] parameters = method.getParameterTypes();
            printParameters(parameters);
            System.out.println(")");
        }
    }
    
    private static void printParameters(Class[] parameters){
        for(int i=0; i<parameters.length; i++){
            System.out.print(parameters[i].getName());
            if(i<parameters.lenth-1)
                System.out.print(",");
        }
    }
}
~~~

<h3>동적 객체 생성(newInstance())</h3>

​	new 연산자를 사용하지 않고 동적으로 객체를 생성할 수 있다.

​	반드시 클래스에 기본 생성자가 존재해야 한다.

​	자바 8버전만 사용하는 듯 [자바 11버전부턴 사용 비권장]

~~~java
public interface Action{
    public void execute();
}
public class SendAction implements Action{
    @Override
    public void execute(){
        System.out.println("데이터를 보냅니다.");
    }
}
public class ReceiveAction implements Action{
    @Override
    public void execute(){
        System.out.println("데이터를 받습니다.");
    }
}
public class NewInstanceClass{
    public static void main(String[] args){
        try{
            Class clazz = Class.forName("Action");
            Action action = (Action) clazz.newInsta
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }catch(InstantiationException e){
            e.printStackTrace();
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }
    }
}
~~~

<h1>String 클래스</h1>

​	문자열을 생성하는 방법과 추출, 비교, 찾기, 분리, 변환 등 처리한다.

<h3>String 생성자</h3>

​	파일의 내용을 읽거나, 네트워크를 통해 받은 데이터는 byte[]배열 이므로 이것을 문자열로 변환하기 위해 사용된다.

~~~java
public class StringClass{
	public static void main(String[] args){
        byte[] bytes = {72,101,108,108,111,32,74,97,118,97};
        
        //byte배열 전체를 String 객체로 생성
        String str1 = new String(bytes);
       	//지정한 문자셋으로 디코딩
        String str2 = new String(bytes, "UTF-8");
		
        //배열의 offset인덱스 위치부터 length까지 String 객체로 생성
        String str3 = new String(bytes,6,4);	//74의 위치부터 4개만
        //지정한 문자셋으로 디코딩
        String str4 = new String(bytes,6,4,"UTF-8");
    }
}

public class KeyboardToStringClass{
    public static void main(String[] args){
        byte[] bytes = new byte[100];
        
        System.out.print("입력: ");
        int readByteNo = System.in.read(bytes);
        
        String str = new String(bytes,0,readByteNo-1);
        System.out.println(str);
    }
}
~~~

<h2>String 메소드</h2>

<h4>문자 추출(charAt())</h4>

~~~java
public class StringCharAtClass{
    public static void main(String[] args){
        String ssn = "010624-1230123";
        char gender = ssn.charAt(7);	//0부터 시작해서 7번째 문자 추출함
    	switch(gender){
            case 1:
            case 3:
                System.out.println("남자입니다.");
            	break;
            case 2:
            case 4:
                System.out.println("여자입니다.");
				break;
        }
    }
} 
~~~

<h4>문자열 비교(equals())</h4>

~~~java
public class StringEqualsClass{
    public static void main(String[] args){
        String strVar1 = new String("신민철");
        String strVar2 = "신민철";
        
        if(strVar1 == strVar2)
            System.out.println("같은 String 객체를 참조");
        else
            System.out.println("다른 String 객체를 참조");
        
        if(strVar1.equals(strVar2))
            System.out.println("같은 문자열을 가짐");
        else
            System.out.println("다른 문자열을 가짐");
    }
}
~~~

<h4>바이트 배열로 변환(getBytes())</h4>

​	네트워크 문자열을 전송하거나 문자열을 암호화할 때 문자열을 바이트 배열로 변환한다.

~~~java
public class StringGetBytesClass{
    public static void main(String[] args){
        String str = "안녕하세요";
        
        byte[] bytes1 = str.getBytes();
        System.out.println("bytes1.length: "+bytes1.length);
        String str1 = new String(bytes1);
        System.out.println("bytes1->String: "+str1);
        
        try{
            byte[] bytes2 = str.getBytes("EUC-KR");
            System.out.println("bytes2.length: "+bytes2.length);
            String str2 = new String(bytes2, "EUC-KR");
            System.out.println("bytes2->String: "+str2);
            
            byte[] bytes2 = str.getBytes("UTF-8");
            System.out.println("bytes2.length: "+bytes2.length);
            String str2 = new String(bytes2, "UTF-8");
            System.out.println("bytes2->String: "+str2);
        }catch(UnsupportedException e){
            e.printStackTrace();
        }
    }
}
~~~

<h4>문자열 찾기(indexOf())</h4>

​	문자열이 시작되는 인덱스를 리턴한다.

​	문자열이 포함되어 있지 않다면 -1을 리턴함

~~~java
public class StringIndexOfClass{
    public static void main(String[] args){
        String subject = "자바 프로그래밍";
        
        int location = subject.indexOf("프로그래밍");
        System.out.println(location);
        
        if(subject.indexOf("자바") != -1)
            System.out.println("자바와 관련된 책이군요.");
        else
            System.out.println("자바와 관련없는 책이군요.");
    }
}
~~~

<h4>문자열 길이(length())</h4>

​	문자열의 길이를 리턴한다.

~~~java
public class StringLengthClass{
    public static void main(String[] args){
        String ssn = "7306241230123";
        int length = ssn.length();
        if(length == 13)
            System.out.println("주민번호 자리수가 맞습니다.");
        else
 			System.out.println("주민번호 자리수가 틀립니다.");           
    }
}
~~~

<h4>문자열 잘라내기(substring())</h4>

~~~java
public class StringSubstingClass{
    public static void main(String[] args){
        String ssn = "880815-1234567";
		//0부터 6번째까지 잘라냄        
        String firstNum = ssn.substring(0,6);
        System.out.pritln(firstNum);
        //7부터 끝까지 잘라냄
        String secondNum = ssn.substring(7);
        System.out.println(secondNum);
    }
}
~~~

<h4>알파벳 소·대문자 변경(toLowerCase(), toUpperCase())</h4>

~~~java
public class StringToLowerCaseUpperCaseClass{
    public static void main(String[] args){
        String str1 = "Java Programming";
        String str2 = "JAVA Programming";
        
        System.out.println(str1.equals(str2));
        
        String lowerStr1 = str1.toLowerCase();
        String lowerStr2 = str2.toLowerCase();
        System.out.println(lowerStr1.equals(lowerStr2));
 		//대소문자 신경안쓰고 비교       
        System.out.println(str1.equalsIgnoreCase(str2));
    }
}
~~~

<h4>문자열 앞뒤 공백 잘라내기(trim())</h4>

~~~java
public class TrimClass{
    public static void main(String[] args){
        String tel1 = " 02";
        String tel2 = "123 ";
        String tel3 = "  1234  ";
        
        String tel = tel1.trim() + tel2.trim() + tel3.trim();
        System.out.println(tel);
    }
}
~~~

<h4>문자열 변환(valueOf())</h4>

​	기본 타입을 문자열로 변환한다.

~~~java
public class ValueOfClass{
    public static void main(String[] args){
        String str1 = String.valueOf(10);
        String str2 = String.valueOf(10.5);
        String str3 = String.valueOf(true);
        
        System.out.println(str1);
        System.out.println(str2);
        System.out.println(str3);
    }
}
~~~

<h1>StringTokenizer 클래스</h1>

​	문자열이 특정 구분자로 연결되어있을 경우, 구분자를 제거하고 문자열을 리턴한다.

<h4>split() 메소드</h4>

​	split() 매소드는 정규표현식을 구분자로 해서 문자열을 분리 후, 문자열 배열로 리턴한다.

~~~java
public class StringSplitClass{
    public static void main(String[] args){
        String text = "홍길동&이수홍,박연수,김자바-김영호";
        
        String[] names = text.split("&|,|-");
        
        for(String name : names)
            System.out.println(name);
    }
}
~~~

<h4>StringTokenizer 클래스</h4>

~~~~java
public class StringTokenizerClass{
    public static void main(String[] args){
        String text = "홍길동/이수홍/박연수";
        
        StringTokenizer st = new StringTokenizer(text, "/");
        int countTokens = st.countTokens();
        for(int i=0; i<countTokens; i++){
            String token = st.nextTokens();
            System.out.println(token);
        }
        
        System.out.println();
        
        st = new StringTokenizer(text,"/");
        while(st.hasMoreTokens()){
            String token = st.nextToken();
            System.out.println(token);
        }
    }
}
~~~~

<h4>StringBuffer, StringBuilder 클래스</h4>

​	임시버퍼를 만들어 문자열을 추가, 수정, 삭제 작업을 할 수 있도록 도와준다.

​	StringBuilder : 단일 스레드 환경에서만 사용하도록 설계되어있다.

​	StringBuffer : 멀티 스레드 환경에서 사용하도록 동기화가 적용되어있다.

~~~java
public class StringBuilderClass{
    public static void main(String[] args){
        StringBuilder sb = new StringBuilder();
        sb.append("Java ");
        sb.append("Program Study");
        System.out.println(sb.toString());
        //4번째 인덱스위치 뒤에 2를 삽입
        sb.insert(4,"2");
        System.out.println(sb.toString());
        //4번째 인덱스위치 문자를 6으로 변경
        sb.setCharAt(4,'6');
        System.out.println(sb.toString());
        //6번째 부터 13전체를 Book문자열로 변경
        sb.replace(6,13,"Book");
        System.out.println(sb.toString());
        //4번째 부터 5번째 전까지 삭제
        sb.delete(4,5);
        System.out.println(sb.toString());
        //총문자열의 길이 알기
        int length = sb.length();
        System.out.println("총 문자수: "+length);
        //버퍼에 있는 것을 String 타입으로 리턴
        String result = sb.toString();
        System.out.println(result);
    }
}
~~~

 <h1>정규 표현식과 Pattern 클래스</h1>

​	문자열 형식을 검증하는 것이다.

<h3>정규 표현식 작성 방법</h3>

​	java.util.regex.Pattern 패키지에 포함되어있다.

<table>
    <tr>
    	<td>기호</td>
    	<td colspan="3">설명</td>
    </tr>
    <tr>
    	<td rowspan="3">[]</td>
        <td rowspan="3">한 개의 문자</td>
        <td>[abc]</td>
        <td>a,b,c 중 하나의 문자</td>
    </tr>
    <tr>
    	<td>[^abc]</td>
        <td>a,b,c 이외의 하나의 문자</td>
    </tr>
    <tr>
    	<td>[a-zA-Z]</td>
        <td>a~z, A~Z 중 하나의 문자</td>
    </tr>
    <tr>
    	<td>\d</td>
        <td colspan="3">한개의 숫자,[0-9]와 동일</td>
    </tr>
    <tr>
    	<td>\s</td>
        <td colspan="3">공백</td>
    </tr>
    <tr>
    	<td>\w</td>
        <td colspan="3">한 개의 알파벳 또는 한 개의 숫자, [a-zA-Z_0-9]와 동일</td>
    </tr>
    <tr>
    	<td>?</td>
        <td colspan="3">없음 또는 한 개</td>
    </tr>
    <tr>
    	<td>*</td>
        <td colspan="3">없음 또는 한 개 이상</td>
    </tr>
    <tr>
    	<td>+</td>
        <td colspan="3">한 개 이상</td>
    </tr>
    <tr>
    	<td>{n}</td>
        <td colspan="3">정확히 n개</td>
    </tr>
    <tr>
        <td>{n,}</td>
        <td colspan="3">최소한 n개</td>
    </tr>
    <tr>
    	<td>{n, m}</td>
        <td colspan="3">n개에서부터 m개까지</td>
    </tr>
    <tr>
    	<td>()</td>
        <td colspan="3">그룹핑</td>
    </tr>
</table>

<h3>Pattern 클래스</h3>

​	Pattern.matches("정규식", "검증할 문자열");

~~~java
public class PatternClass{
    public static void main(String[] args){
        String regExp = "(02|010)-\\d{3,4}-\\d{4}";
        String data = "010-123-4567";
        boolean result = Pattern.matches(regExp,data);
        if(result)
            System.out.println("정규식과 일치합니다.");
        else
            System.out.println("정규식과 일치하지 않습니다.");
        
        regExp = "\\w+@\\w+\\.\\w+(\\.\\w+)?";
        data = "angel@navercom";
        result = Pattern.matches(regExp, data);
        if(result)
            System.out.println("정규식과 일치합니다.");
		else
            System.out.println("정규식과 일치하지 않습니다.");
    }
}
~~~

<h1>Arrays 클래스</h1>

​	Arrays 클래스는 배열 조작기능을 가지고 있다.

​	배열 조작 = 배열의 복사, 항목 정렬, 항목 검색과 같은 기능

<h3>배열 복사</h3>

~~~java
public class ArrayCopyClass{
    public static void main(String[] args){
        char[] arr1 = {'J','A','V','A'};
        
        //방법 1
        char[] arr2 = Arrays.copyOf(arr1,arr1.length);
        System.out.println(Arrays.toString(arr2));
        
        //방법 2
        //arr1[1] ~ arr1[2]를 arr3[0] ~ arr3[1]로 복사
        char[] arr3 = Arrays.copyOfRange(arr1,1,3);
        System.out.println(Arrays.toString(arr3));
        
        
        //방법 3
        char[] arr4 = new char[arr1.length];
        System.arraycopy(arr1,0,arr4,0,arr1.length);
        for(int i=0; i<arr4.length; i++)
            System.out.println("arr4["+i+"]="+arr4);
            
    }
}
~~~

<h3>배열 항목 비교</h3>

~~~java
public class EqualsClass{
    public static void main(String[] args){
        int[][] original = {
            {1,2},
            {3,4}
        };
        
        //얕은 복사 후 비교
        System.out.println("[얕은 복사 후 비교]");
        int[][] arryInt1 = Arrays.copyOf(original,original.length);
        System.out.println("배열 번지 비교: "+original.equals(arryInt1));
        System.out.println("1차 배열 항목값 비교: "+Arrays.equals(original,arrayInt1));
        System.out.println("중첩 배열 항목값 비교: "+Arrays.deepEquals(original,arrayInt1));
        
        //깊은 복사 후 비교
        System.out.println("[깊은 복사 후 비교]");
    	int[][] arryInt2 = Arrays.copyOf(original,original.length);
        arryInt2[0] = Arrays.copyOf(original[0], original[0].length);
        arryInt2[1] = Arrays.copyOf(original[1], original[1].length);
        System.out.println("배열 번지 비교: "+original.equals(arryInt2));
        System.out.println("1차 배열 항목값 비교: "+Arrays.equals(original,arrayInt2));
        System.out.println("중첩 배열 항목값 비교: "+Arrays.deepEquals(original,arrayInt2));
    }
}
~~~

<h3>배열 항목 정렬</h3>

​	사용자 정의 클래스일 경우, Comparable<T> 인터페이스를 구현받아야 한다.

<dl>
    <dt>compareTo() 메소드 리턴값</dt>
    <dt>1. 오름차순일 경우</dt>
    <dd>자신이 매개값보다 낮은 경우 양수</dd>
    <dd>자신이 매개값이랑 같은 경우 0</dd>
    <dd>자신이 매개값보다 높은 경우 음수</dd>
    <dt>2. 내림차순일 경우</dt>
    <dd>자신이 매개값보다 낮은 경우 음수</dd>
    <dd>자신이 매개값이랑 같은 경우 0</dd>
    <dd>자신이 매개값보다 높은 경우 양수</dd>
</dl>

~~~java
public class Member implements Comparable<Member>{
    private String name;
    public Member(String name){
        this.name = name;
    }
    @Override
    public int compareTo(Member o){
        return name.compareTo(o.name);
    }
    public String getName(){
        return name;
    }
}

//배열 비교
public class SortClass{
    public static void main(String[] args){
        int[] scores = {99,97,98};
        Arrays.sort(scores);
        for(int=0; i<scores.length; i++)
            System.out.println("scores["+i+"]="+scores[i]);
        System.out.println("---------------------");
        
        String[] names = {"홍길동","박동수","김자바"};
        Arrays.sort(names);
        for(int i=0; i<names.length; i++)
            System.out.println("name["+i+"]="+names[i]);
        System.out.println("-----------------------");
        
        Member m1 = new Member("홍길동");
        Member m2 = new Member("박동수");
        Member m3 = new Member("김자바");
        Member[] members = {m1,m2,m3};
        Arrays.sort(members);
        for(int i=0; i<members.length; i++)
            System.out.println("members["+i+"].name="+members[i].getName());
        
    }
}
~~~

<h3>배열 항목 검색</h3>

​	배열에서 특정 값이 위치한 인덱스를 얻는 것을 배열 검색이라고 한다.

​	binarySearch()는 배열이 정렬 되어 있어야 검색이 가능하다.

~~~java
public class Member implements Comparable<Member>{
    private String name;
    public Member(String name){
        this.name = name;
    }
    @Override
    public int compareTo(Member o){
        return name.compareTo(o.name);
    }
    public String getName(){
        return name;
    }
}

public class SearchClass{
    public static void main(String[] args){
        int[] scores = {99,97,98};
        Arrays.sort(scores);
        int index = Arrays.binarySearch(scores, 99);
        System.out.println("찾은 인덱스: "+index);
        
        String[] names = {"홍길동","박동수","김자바"};
        Arrays.sort(names);
        index = Arrays.binarySearch(names, "홍길동");
        System.out.println("찾은 인덱스: "+index);
        
        Member m1 = new Member("홍길동");
        Member m2 = new Member("박동수");
        Member m3 = new Member("김자바");
        Member[] members = {m1,m2,m3};
        Arrays.sort(members);
        index = Arrays.binarySearch(members, m1);
        System.out.println("찾은 인덱스: "+index);
    }
}
~~~

<h2>Wrapper(포장) 클래스</h2>

​	포장 객체의 특징은 기본 타입의 값을 내부에 두고 포장한다. 외부에서 변경할 수 없다.

​	java.lang 패키지에 포함되어 있다.

<table>
    <tr>
    	<th>기본 타입</th>
        <th>포장 클래스</th>
    </tr>
    <tr>
    	<td>byte</td>
        <td>Byte</td>
    </tr>
    <tr>
    	<td>char</td>
        <td>Character</td>
    </tr>
    <tr>
    	<td>short</td>
        <td>Short</td>
    </tr>
    <tr>
    	<td>int</td>
        <td>Integer</td>
    </tr>
    <tr>
    	<td>long</td>
        <td>Long</td>
    </tr>
    <tr>
    	<td>float</td>
        <td>Float</td>
    </tr>
    <tr>
    	<td>double</td>
        <td>Double</td>
    </tr>
    <tr>
    	<td>boolean</td>
        <td>Boolean</td>
    </tr>
</table>

<h3>박싱(Boxing)과 언박싱(Unboxing)</h3>

​	기본타입의 값을 포장 객체로 만드는 것을 박싱이라고 한다.

​	포장객체에서 기본 타입의 값을 얻어내는 과정을 언박싱이라고 한다.

~~~java
public class BoxingUnBoxingClass{
    public static void main(String[] args){
        //Boxing
        Integer obj1 = new Integer(100);
        Integer obj2 = new Integer("200");
        Integer obj3 = new Integer.valueOf("300");
        
        //Unboxing
        int value1 = obj1.intValue();
        int value2 = obj2.intValue();
        int value3 = obj3.intValue();
        
        System.out.println(value1);
        System.out.println(value2);
    	System.out.println(value3);
    }
}
~~~

<h4>자동 박싱과 언박싱</h4>

​	자동 박싱 : 포장 클래스에 기본타입의 값이 대입될 경우

​	자동 언박싱 : 기본타입에 포장 클래스가 대입되는 경우

​	컬렉션 객체에 int 값을 저장하면 자동 박싱이 일어난다.

~~~java
List<Integer> list = new ArrayList<Integer>();
list.add(200); //자동 박싱
~~~



~~~java
public class AutoBoxingUnBoxingClass{
    //자동 Boxing
    Integer obj = 100;
    System.out.println("value: "+obj.intValue());
    
    //대입 시 자동 Unboxing
    int value = obj;
    System.out.println("value: "+value);
    
    //연산 시 자동 Unboxing
    int result = obj + 100;
    System.out.println("result: "+result);
}
~~~

<h4>문자열을 기본 타입 값으로 변환</h4>

~~~java
public class StringToPrimitiveValueClass{
    public static void main(String[] args){
        int value1 = Integer.parseInt("10");
        double value2 = Double.parseDouble("3.14");
        boolean value3 = Boolean.pareBoolean("true");

        System.out.println("value1: "+value1);
        System.out.println("value2: "+value2);
        System.out.println("value3: "+value3);
    }
}
~~~

<h4>포장 값 비교</h4>

​	포장 객체는 내부의 값을 비교하기 때문에 ==, != 연산자를 사용할 수 없다.

​	equals() 메소드로 내부의 값을 비교하는 것이 좋다.

~~~java
public class ValueCompareClass{
    public static void main(String[] args){
        System.out.println("[-128~127 초과값일 경우]");
        Integer obj1 = 300;
        Integer obj2 = 300;
        System.out.println("==결과: "+(obj1 == obj2));
        System.out.println("언박싱후 ==결과: "+(obj1.intValue()==obj2.intValue()));
        System.out.println("equals() 결과: "+obj1.equals(obj2));
        
        System.out.println("[-128~127 범위값일 경우]");
        Integer obj3 = 10;
        Integer obj4 = 10;
        System.out.println("==결과: "+(obj3 == obj4));
        System.out.println("언박싱후 ==결과: "+(obj3.intValue()==obj4.intValue()));
        System.out.println("equals() 결과: "+obj3.equals(obj4));
    }
}
~~~

 <h1>Math, Random 클래스</h1>

<h3>Math 클래스</h3>

​	Math.random() 메소드는 0보다는 크거나 같고, 1보다는 작은 값을 리턴한다.

~~~java
public class MathClass{
    public static void main(String[] args){
        int v1 = Math.abs(-5);
        double v2 = Math.abs(-3.14);
        System.out.println("v1="+v1);
        System.out.println("v2="+v2);
        
        double v3 = Math.ceil(5.3);
        double v4 = Math.ceil(-5.3);
        System.out.println("v3="+v3);
        System.out.println("v4="+v4);
        
        double v5 = Math.floor(5.3);
        double v6 = Math.floor(-5.3);
        System.out.println("v5="+v5);
        System.out.println("v6="+v6);
        
        int v7 = Math.max(5,9);
        double v8 = Math.max(5.3,2.5);
        System.out.println("v7="+v7);
        System.out.println("v8="+v8);
        
        int v9 = Math.min(5,9);
        double v10 = Math.min(5.3,2.5);
        System.out.println("v9="+v9);
        System.out.println("v10="+v10);
        
        double v11 = Math.random();
        System.out.println("v11="+v11);
        
        double v12 = Math.rint(5.3);
        double v13 = Math.rint(5.7);
        System.out.println("v12="+v12);
        System.out.println("v13="+v13);
        
        long v14 = Math.round(5.3);
        long v15 = Math.round(5.7);
        System.out.println("v14="+v14);
        System.out.println("v15="+v15);
        
        double value = 12.3456;
        double temp1 = value * 100;
        long temp2 = Math.round(temp1);
        double v16 = temp2 / 100.0;
        System.out.println("v16="+v16);
    }
}

public class MathRandomClass{
    public static void main(String[] args){
        int num = (int)(Math.random*6)+1;
        System.out.println("주사위 눈: "+num);
    }
}
~~~

<h3>Random 클래스</h3>

​	Random 클래스는 종자값(seed)을 설정할 수 있다.

​	종자값은 알고리즘에 사용되는 값으로 종자갑이 같으면 같은 난수를 얻는다.

~~~java
public class RandomClass{
    public static void main(String[] args){
        int[] selectNumer = new int[6];
        Random random = new Random(3);
        System.out.print("선택 번호: ");
        for(int i=0; i<6; i++){
            selectNumber[i] = random.nextInt(45)+1;
            System.out.print(selectNumber[i]+" ");
        }
        System.out.println();
        int[] winningNumber = new int[6];
        random = new Random(5);
        System.out.print("당첨 번호: ");
        for(int i=0; i<6; i++){
            winningNumber[i] = random.nextInt(45)+1;
            System.out.print(winningNumber[i]+" ");
        }
        System.out.println();
        
        Arrays.sort(selectNumber);
        Arrays.sort(winningNumber);
        boolean result = Arrays.equals(selectNumber, winningNumber);
        if(result)
            System.out.println("1등에 당첨되셨습니다.");
        else
            System.out.println("당첨되지 않았습니다.");
    }
}
~~~

<h1>Date, Calendar 클래스</h1>

<h3>Date 클래스</h3>

​	java.util 패키지에 포함되어 있다.

​	날짜를 표현하는 클래스이다.

~~~java
public class DateClass{
    public static void main(String[] args){
        Date now = new Date();
        String strNow1 = now.toString();
        System.out.println(strNow1);
        
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");
        String strNow2 = sdf.format(now);
        System.out.println(strNow2);
    }
}
~~~

<h3>Calendar 클래스</h3>

​	달력을 표현한 클래스이다.

​	Calendar 클래스는 추상 클래스이므로 new 연산자를 사용해서 인스턴스할 수 없다.

~~~java
public class CalendarClass{
    public static void main(String[] args){
		Calendar now = Calendar.getInstance();
        
        int year = now.get(Calendar.YEAR);
        int month = now.get(Calendar.MONTH)+1;
        int day = now.get(Calendar.DAY_OF_MONTH);
        
        int week = now.get(Calendar.DAY_OF_WEEK);
        String strWeek = null;
        switch(week){
            case Calendar.MONDAY:
                strWeek = "월";
                break;
            case Calendar.TUESDAY:
                strWeek = "화";
                break;
            case Calendar.WEDNESDAY:
                strWeek = "수";
                break;
            case Calendar.THURSDAY:
                strWeek = "목";
                break;
            case Calendar.FRIDAY:
                strWeek = "금";
                break;
            case Calendar.SATURDAY:
                strWeek = "토";
                break;
            default:
                strWeek = "일";
        }
        
        int amPm = now.get(Calendar.AM_PM);
        String strAmPm = null;
        if(amPm == Calendar.AM)
            strAmPm = "오전";
		else
            strAmPm = "오후";
        
        int hour = now.get(Calendar.HOUR);
        int minute = now.get(Calendar.MINUTE);
        int second = now.get(Calendar.SECOND);
        
        System.out.print(year+"년 ");
        System.out.print(month+"월 ");
        System.out.print(day+"일 ");
        System.out.print(strWeek+"요일 ");
        System.out.println(strAmPm+" ");
        System.out.print(hour+"시 ");
        System.out.print(minute+"분 ");
        System.out.print(second+"초 ");
    }
}
~~~

<h1>Format 클래스</h1>

​	java.text 패키지에 포함되어 있다.

<h3>숫자 형식 클래스(DecimalFormat)</h3>

~~~java
public class DecimalFormatClass{
    public static void main(String[] args){
        double num = 1234567.89;
        
        DecimalFormat df = new DecimalFormat("0");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("0.0");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("0000000000.00000");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("#");
        System.out.println(df.format(num));
            
        df = new DecimalFormat("#.#");
        System.out.println(df.format(num));
            
        df = new DecimalFormat("##########.#####");
        System.out.println(df.format(num));
            
        df = new DecimalFormat("#.0");
        System.out.println(df.format(num));
            
        df = new DecimalFormat("+#.0");
        System.out.println(df.format(num));
            
        df = new DecimalFormat("-#.0");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("#,###.0");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("0.0E0");
        System.out.println(df.format(num));
        
    	df = new DecimalFormat("+#,### ; -#,###");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("#.# %");
        System.out.println(df.format(num));
        
        df = new DecimalFormat("\u00A4 #,###");
        System.out.println(df.format(num));
    }
}
~~~

<h3>날짜 형식 클래스(SimpleDateFormat)</h3>

~~~java
public class SimpleDateFormatClass{
    public static void main(String[] args){
        Date now = new Date();
        
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        System.out.println(sdf.format(now));
        
        sdf = new SimpleDateFormat("yyyy년 MM월 dd일");
        System.out.println(sdf.format(now));
        
		sdf = new SimpleDateFormat("yyyy.MM.dd a HH:mm:ss");
        System.out.println(sdf.format(now));
        
        sdf = new SimpleDateFormat("오늘은 E요일");
        System.out.println(sdf.format(now));
        
        sdf = new SimpleDateFormat("올해의 D번째 날");
        System.out.println(sdf.format(now));
        
        sdf = new SimpleDateFormat("이달의 d번째 날");
        System.out.println(sdf.format(now));
    }
}
~~~

<h3>문자열 형식 클래스(MessageFormat)</h3>

​	데이터를 파일에 저장하거나, 네트워크로 전송할 때, 데이터베이스 SQL문을 작성할 때 등에 사용된다.

​	문자열에 데이터가 들어갈 자리를 표시해주고, 프로그램이 실행하면서 동적으로 데이터를 사입해 문자열을 완성시킨다.

~~~java
public class MessageFormatClass{
    public static void main(String[] args){
        String id = "java";
        String name = "김자바";
        String tel = "010-1111-3222";
        String text = "회원 ID: {0} \n회원 이름: {1} \n회원 전화: {2}";
        String result = MessageFormat.format(text,id,name,tel);
        System.out.println(result);
        Sysetm.out.println();
        
        String sql = "insert into member values({0}, {1}, {2})";
        Object[] arguments = {"java","김자바","010-1111-2222"};
        String result2 = MessageFormat.format(sql,arguments);
        System.out.println(result2);
    }
}
~~~



<h1>java.time 패키지</h1>

​	Calendar 클래스는 날짜와 시간 정보를 얻기에는 충분하지만 날짜와 시간을 조작하거나 비교하는 기능이 불가능하다.

<h3>LocalDate</h3>

​	로컬 날짜 클래스로 날짜 정보만을 저장한다.

~~~java
LocalDate currDate = LocalDate.now();
LocalDate targetDate = LocalDate.fo(int year, int month, int dayOfMonth);
~~~

<h3>LocalTime</h3>

​	로컬 시간 클래스로 시간정보만을 저장한다.

~~~java
LocalTime currTime = LocalTime.now();
LocalTime targetTime = LocalTime.of(int hour, int minute, int second, int nanoOfSecond);
~~~

<h3>LocalDateTime</h3>

​	LocalDate + LocalTime을 결합한 클래스이다.

​	날짜와 시간 모두 저장가능하다.

~~~java
LocalDateTime currDateTime = LocalDateTime.now();
LocalDateTime targetTime = LocalDateTime.of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanoOfSecond);
~~~

<h3>ZonedDateTime</h3>

​	ISO-8601 달력 시스템에서 정ㅇ의하고 있는 타임존의 날짜와 시간을 저장하는 클래스이다.

~~~java
ZonedDateTime utcDateTime = ZonedDateTime.now(ZoneId.of("UTC"));
ZonedDateTime londonDateTime = ZonedDateTime.now(ZoneId.of("Europe/London"));
ZonedDateTime seoulDateTime = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
~~~

<h3>Instant</h3>

​	특정 시점의 타임스탬프로 사용된다.

​	주로 특정한 두 시점 간의 시간적 우선순위를 따질 때 사용한다.

~~~java
Instant instant1 = Instant.now();
Instant instant2 = Instant.now();
if(instant1.isBefore(instant2)) System.out.println("instant1이 빠릅니다.");
else if(instant1.isAfter(instant2)) System.out.println("instant1이 늦습니다.");
else System.out.println("동일한 시간대입니다.");
~~~



~~~java
public class DateTimeInfoClass{
    public static void main(String[] args){
        LocalDateTime now = LocalDateTime.now();
        System.out.println(now);
        
        String strDateTime = now.getYear() + "년 ";
        strDateTime += now.getMonthValue() + "월 ";
        strDateTime += now.getDayOfWeek() + " ";
        strDateTime += now.getHour() + "시 ";
        strDateTime += now.getMinute() + "분 ";
        strDateTime += now.getSecond() + "초 ";
        strDateTime += now.getNano() + "나노초";
        System.out.println(strDateTime+"\n");
        
        LocalDate nowDate = now.toLocalDate();
        if(nowDate.isLeapYear())
            System.out.println("올해는 윤년: 2월은 29일까지 있습니다.\n");
        else
            System.out.println("올해는 평년: 2월은 28일까지 있습니다.\n");
    
    	//협정 세계시와 존오프셋
        ZonedDateTime utcDateTime = ZonedDateTime.now(ZonedId.of("UTC"));
        System.out.println("협정 세계시: "+utcDateTime);
        ZonedDateTime seoulDateTime = ZonedDateTime.now(ZonedId.of("Asia/Seoul"));
        System.out.println("서울 타임존: "+seoulDateTime);
        ZoneId seoulZoneId = seoulDateTime.getZone();
        System.out.println("서울 존아이디: "+seoulZoneId);
        ZoneOffset seoulZoneOffset = seoulDateTime.getOffset();
        System.out.println("서울 존오프셋: "+seoulZoneOffset+"\n");
    }
}
~~~

<h1>날짜와 시간을 조작하기</h1>

<h3>빼기와 더하기</h3>

~~~java
public class DateTimeOperationClass{
    public static void main(String[] args){
        LocalDateTime now = LocalDateTime.now();
        System.out.println("현재시: "+now);
        
        LocalDateTime targetDateTime = now
            .plusYears(1)
            .minusMonths(2)
            .plusDays(3)
            .plusHours(4)
            .minusMinutes(5)
            .plusSeconds(6);
        System.out.println("연산후: "+targetDateTime);
    }
}
~~~

<h3>변경하기</h3>

~~~java
public class DateTimeChangeClass{
    public static void main(String[] args){
        LocalDateTime now = LocalDateTime.now();
        System.out.println("현재: "+now);
        
        LocalDateTime targetDateTime = null;
        
        targetDateTime = now
            .withYear(2024)
            .withMonth(10)
            .withDayOfMonth(5)
            .withHour(13)
            .withMinute(30)
            .withSecond(20);
        System.out.println("직접 변경: "+targetDateTime);
        
        targetDateTime = now.with(TemporalAdjusters.firstInMonth(DayOfWeek.MONDAY));
        System.out.pritnln("이번 달의 첫 월요일: "+targetDateTime);
        targetDateTime = now.with(TemporalAdjusters.next(DayOfWeek.MONDAY));
        System.out.println("돌아오는 월요일: "+targetDateTime);
        targetDateTime = now.with(TemporalAdjusters.previous(DayOfWeek.MONDAY));
        System.out.println("지난 월요일: "+targetDateTime);
    }
}
~~~

<h3>날짜와 시간을 비교하기</h3>

~~~java
public class DateTimeCompareClass{
    public static void main(String[] args){
        LocalDateTime startDateTime = LocalDateTime.of(2023,1,1,9,0,0);
        System.out.println("시작일: "+startDateTime);
        
        LocalDateTime endDateTime = LocalDateTime.of(2024,3,31,18,0,0);
        System.out.println("종료일: "+endDateTime);
        
        if(startDateTime.isBefore(endDateTime))
            System.out.println("진행 중입니다.\n");
        else if(startDateTime.isEqual(endDateTime))
            System.out.println("종료합니다.\n");
        else if(startDateTime.isAfter(endDateTime))
            System.out.println("종료했습니다.\n");
        
        System.out.println("[종료까지 남은 시간]");
        long remainYear = startDateTime.until(endDateTime,ChronoUnit.YEARS);
        long remainMonth = startDateTime.until(endDateTime,ChronoUnit.MONTHS);
        long remainDay = startDateTime.until(endDateTime,ChronoUnit.DAYS);
        long remainHour = startDateTime.until(endDateTime,ChronoUnit.HOURS);
        long remainMinute = startDateTime.until(endDateTime,ChronoUnit.MINUTES);
        long remainSecond = startDateTime.until(endDateTime,ChronoUnit.SECONDS);
        
        remainYear = ChronoUnit.YEARS.between(startDateTime, endDateTime);
        remainMonth = ChronoUnit.MONTHS.between(startDateTime, endDateTime);
        remainDay = ChronoUnit.DAYS.between(startDateTime, endDateTime);
        remainHour = ChronoUnit.HOURS.between(startDateTime, endDateTime);
        remainMinute = ChronoUnit.MINUTES.between(startDateTime, endDateTime);
        remainSecond = ChronoUnit.SECONDES.between(startDateTime, endDateTime);
    
    	System.out.println("남은 해: "+remainYear);
        System.out.println("남은 달: "+remainMonth);
        System.out.println("남은 일: "+remainDay);
        System.out.println("남은 시간: "+remainHour);
        System.out.println("남은 분: "+remainMinute);
        System.out.println("남은 초: "+remainSecond);
        
        System.out.println("[종료까지 남은 기간]");
        Period period = 			
        Period.between(startDateTime.toLocalDate(),endDateTime.toLocalDate());
   		System.out.print("남은 기간: "+period.getYears()+"년 ");
        System.out.print(period.getMonths()+"달 ");
        System.out.println(period.getDays()+"일\n");
        
        Duration duration =
       	Duration.between(startDate.toLocalTime(), endDateTime.toLocalDate());
        System.out.println("남은 초: "+duration.getSeconds());
    }
}
~~~

<h1>파싱과 포멧팅</h1>

<h3>파싱(Parsing) 메소드</h3>

​	날짜와 시간 정보가 포함된 문자열을 파싱해서 날짜와 시간을 생성하는 두 개의 parse() 정적메소드이다.

~~~java
public class DateTimeParsingClass{
    public static void main(String[] args){
        DateTimeFormatter formatter;
        LocalDate localDate;
		
        localDate = LocalDate.parse("2024-05-21");
        System.out.println(localDate);
        
        formatter = DateTimeFormatter.ISO_LOCAL_DATE;
        localDate = LocalDate.parse("2024-05-21",formatter);
        System.out.println(localDate);
        
        formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
        localDate = LocalDate.parse("2024/05/21",formatter);
        System.out.println(localDate);
        
        formatter = DateTimeFormatter.ofPattern("yyyy.MM.dd");
        localDate = LocalDate.parse("2024.05.21",formatter);
        System.out.println(localDate);
    }
}
~~~

	<h3>포멧팅(Formatting) 메소드</h3>

~~~java
public class DateTimeFormatClass{
    public static void main(String[] args){
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter dateTimeFormatter =
        DateTimeFormatter.ofPattern("yyyy년 M월 d일 a h시 m분");
        String nowString = now.format(dateTimeFormatter);
        System.out.println(nowString);
    }
}
~~~




