# Lambda学习经验
//1.Lambda表达式中可以访问成员变量也可以访问局部变量，但是对于局部变量默认变为final修饰，不可再改变。以下是代码
    public class Test1{
      private static int a;
      private int b;
      public static void main(String[] args) {
      private static int j;
      private int k;      //main是静态方法，不能访问非静态变量k
      int num=10;
      Action5 act=(i)->System.out.println("j+K+i+num="+(j+i+num));
      act.run(2);
    }
    interface Action5{
      public void run(int i);
    }
//2.类名::静态方法，类名::非静态方法
    public class LambdaTest2 {
      public static void main(String[] args) {
        LambdaTest2 t=new LambdaTest2();
        //::引用Integer中的静态方法
        Action2 a1=Integer::toBinaryString;    //toBinaryString返回又字符串表示的二进制化的无符号整数，如10101
        System.out.println(a1.run(4));
        
        //::引用非静态方法，其中引用非静态方法必须先创建对象
        Action2 a2=t::test;
        System.out.println(a2.run(4));
      }
      public String test(int i){
        return "i="+i;
      }
     }
     @FunctionalInterface   //标注：有且仅有一个抽象方法
     interface Action2{
        public String run(int Integer);
     }
     
//3.对于泛型的Action,可以用 类名::非静态方法，其中非静态方法必须不带参数
    public class LambdaTest3 {
      public static void main(String[] args) {
        Action<Model> a2=Model::test1;
        a2.run(m);
        //引用带参数的方法需要创建对象m,并且参数应与Action中参数一致
        Action<Model> a3=m::test2;
        a3.run(m);
      }
    }
    interface Action<T>{
      public void run(T t);
    }
    class Model{
      public void test1(){
        System.out.println("test1");
      }
       public void test2(Model a){
        System.out.println("test2");
      }
    }
    
//4.引用构造函数
    public class LambdaTest4 {
	    public static void main(String[] args) {
        Action4Creater creater=Action4::new;
        Action4 a4=creater.create("zhangsan");
        a4.say();
      }
    }
    interface ActionCreater{
        public Action4 create(String name);
      }
    class Action5{
        private String name;
        public Action4(String name){
          this.name=name;
        }
        public void say(){
          System.out.println("name="+name);
        }
      }

//5.对于线程可以这样做
new Thread(()->System.out.println("first lambda")).start();

//6.可以在集合的forEach方法中这样做
    List<String> features=Arrays.asList("h","S","C","k","L");
    features.forEach(n->System.out.print(n+" "));
//甚至可以这样
		features.forEach(System.out::print);
    
//7.predicate的使用
		List<String> languages=Arrays.asList("Java","Scala","C++","Haskell","Lisp");
		System.out.println("Languages which starts with J:");
		filter1(languages,(str)->((String) str).startsWith("J"));
    //自己写的过滤器
    public static void filter1(List<String> names,Predicate<String> condition){
		for(String name:names){
			if(condition.test(name)){
				System.out.println(name+" ");
			}
		}
		//stream中的filter的使用
		names.stream().filter((name)->(condition.test(name))).forEach((name)->{
			System.out.println(name+" ");
		});
	}
 //8. predicate接口的方法 test,negate,or,and,isEqual
    Predicate<String> c1=(str)->str.startsWith("J");		
		Predicate<String> c2=(str)->str.length()==4;
    
    filter1(languages,c1.and(c2));
		filter1(languages,c2.negate());
		filter1(languages,c1.or(c2));
		filter1(languages,Predicate.isEqual("Java"));
    //判断两个字符是否相等
    Predicate.isEqual("hello").test("world");
    //运用predicate的排序
    Collections.sort(names,(s1,s2)->s1.compareTo(s2));
 
  
