---
title: Java小练习--简单的学生管理系统
categories: 
    - [学习备忘笔记]
single_column: true
cover: https://pic-bed-6vd.pages.dev/img/fm9.webp
banner:
  type: img
  bgurl: https://pic-bed-6vd.pages.dev/img/bg11.webp
  banner_text: 
---
## 首先创建了一个StudentSystemLogin类，它实现了一个学生管理系统的登录模块，包括用户注册、登录和忘记密码的功能。

```java
import java.util.ArrayList;
import java.util.Random;
import java.util.Scanner;

public class StudentSystemLoginUpper {
//   存储用户信息
    static ArrayList<User> user = new ArrayList<>();
    static {
        User u1 = new User("admin","123456","375664874451202354","13512345678");
        User u2 = new User("teacher","123456","154564654781202354","17612345678");
        user.add(u1);
        user.add(u2);   
    }

//   使用 while (true) 循环，持续显示菜单并等待用户输入操作选项
    public static void main(String[] args) {
        while (true) {
            System.out.println("--------欢迎来到学生管理系统--------");
            System.out.println("请选择操作：1、登录   2、注册   3、忘记密码  4、关闭页面");
            Scanner sc = new Scanner(System.in);
            switch(sc.next()){
                case "1" -> login(user);
                case "2" -> register(user);
                case "3" -> forgetPwd(user);
                case "4" -> {
                    System.out.println("关闭页面");
                    System.exit(0);
                }
                default -> System.out.println("请输入正确选项");
            }
        }
    }

    //注册功能
    public static void register(ArrayList<User> user){
        //用户名
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入注册的用户名");
        loop1:while (true) {
//            键盘录入用户名
            String username = sc.next();
//            判断用户是否存在
            boolean falg = containsusername(user,username);
            if(falg){
                System.out.println("该用户名已存在，请重新输入");
            }else{
//                验证用户名格式是否正确：用户名长度在3~15位之间
                int lent = username.length();
                if(lent >= 3 && lent <= 15){
//                        验证用户名格式：只能为字母加数字的组合，不能为纯数字
                        boolean flag = checkUsername(username);
                        if(flag){
                            User u = new User();
                            user.add(u);
                            user.get(user.size()-1).setUsername(username);
                            System.out.println("用户名注册成功");
                            break;
                        }else{
                            System.out.println("只能是字母加数字的组合,不能为纯数字");
                            System.out.println("请重新输入");
                        }
                }
                else{
                    System.out.println("用户名长度应在3~15位之间");
                    System.out.println("请重新输入");
                }
            }
        }

        //密码
        System.out.println("请输入注册的密码");
        while (true) {
            String pwd1 = sc.next();
            System.out.println("请再次输入相同的密码");
            String pwd2 = sc.next();
            if(pwd1.equals(pwd2)){
                user.get(user.size()-1).setPassword(pwd2);
                System.out.println("密码注册成功");
                break;
            }else{
                System.out.println("两次密码输入不相同，请重新输入");
            }
        }

        //身份证号码
        System.out.println("请输入您的身份证号码");
        loop2:while (true) {
            String id = sc.next();
            if(id.length()==18){
                if(id.charAt(0)=='0'){
                    System.out.println("身份证号码不能以0开头，请重新输入");

                }else{
                    for(int i =0;i<16;i++){
                        char c = id.charAt(i);
                        if(c>='0'&&c<='9'){
                            if((id.charAt(17)>='0'&&id.charAt(17)<='9')||(id.charAt(17)=='x'||id.charAt(17)=='X')){
                                user.get(user.size()-1).setId(id);
                                System.out.println("身份证号码注册成功");
                                break loop2;
                            }else{
                                System.out.println("身份证最后一位只能为数字或x或X，请重新输入");
                                break;
                            }
                        }else{
                            System.out.println("身份证号码前17位不完全为数字，请重新输入");
                        }
                    }
                }
            }else{
                System.out.println("身份证号码不为18位，请重新输入");
            }
        }

        //手机号
        System.out.println("请输入您的手机号");
        while (true) {
            String phone = sc.next();
            if(phone.length()==11){
                if(phone.charAt(0)=='0'){
                    System.out.println("手机号首位不能为0，请重新输入");
                }else{
                    boolean f = allIsNumber(phone);
                    if(f){
                        user.get(user.size()-1).setPhoneNumber(phone);
                        System.out.println("手机号注册成功");
                        break;
                    }else{
                        System.out.println("手机号11位必须全部为数字,请重新输入");
                    }
                }
            }else{
                System.out.println("手机号长度应为11位，请重新输入");
            }
        }

    }

    //登录功能
    public static void login(ArrayList<User> user){
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名");
        String username = sc.next();
        boolean flag = containsusername(user,username);
        if(flag){
            System.out.println("请输入密码");
            int i = 0;
            loop3:while (true) {
                String password = sc.next();
                int index = getIndex(user,username);
                if(password.equals(user.get(index).getPassword())){
                    System.out.println("请输入验证码");
                    while (true) {
                        String ryan = yanZhengMa();
                        System.out.println("生成验证码为"+ryan);
                        String yanzhengma = sc.next();
                        if(yanzhengma.equalsIgnoreCase(ryan)){
                            System.out.println("登录成功");
                            StudentSystem sss = new StudentSystem();
                            sss.startStudentSystem();
                            break loop3;
                        }else{
                            System.out.println("验证码输入错误，请重新输入。");
                        }
                    }
                }else{
                    System.out.println("密码输入错误。请重新输入");
                }
                i++;
                if(i==3){
                    System.out.println("密码输入错误超过三次，已退出");
                    return;
                }
            }
        }else{
            System.out.println("用户名未注册，请先注册");
            return;
        }
    }

    //忘记密码功能
    public static void forgetPwd(ArrayList<User> user){
        System.out.println("请输入要修改密码的用户名称");
        Scanner sc = new Scanner(System.in);
        String username = sc.next();
        boolean flag = containsusername(user,username);
        if(flag) {
            System.out.println("请输入身份证号码");
            String id = sc.next();
            int index = getIndex(user,username);
            if(id.equals(user.get(index).getId())){
                System.out.println("身份证验证成功，请输入手机号");
                String number = sc.next();
                if(number.equals(user.get(index).getPhoneNumber())){
                    System.out.println("手机号验证成功。");
                    System.out.println("请输入要修改的密码");
                    while (true) {
                        String password = sc.next();
                        if(password.equals(user.get(index).getPassword())){
                            System.out.println("修改密码不能与原密码相同，请重新输入。");
                        }else{
                            user.get(getIndex(user,username)).setPassword(password);
                            System.out.println("密码修改成功");
                            break;
                        }
                    }
                }else{
                    System.out.println("手机号验证失败，修改失败。");
                    return;
                }
            }else{
                System.out.println("身份证验证失败，修改失败。");
                return;
            }
        }else{
            System.out.println("该用户未注册");
            return;
        }
    }

    //判断用户名是否唯一
    public static boolean containsusername(ArrayList<User> user, String username){
        for(int i = 0; i<user.size(); i++){
            if(user.get(i).getUsername().equals(username)){
                return true;
            }
        }
        return false;
    }

    //根据用户名查找集合的索引
    public static int getIndex(ArrayList<User> user,String username){
        for(int i = 0; i<user.size(); i++){
            if(user.get(i).getUsername().equals(username)){
                return i;
            }
        }
        return -1;
    }

    //判断手机号11位全部为数字
    public static boolean allIsNumber(String number){
        for(int i = 0;i<number.length();i++){
            char c = number.charAt(i);
            if(!(c>='0'&&c<='9')){
                return false;
            }
        }
        return true;
    }

    //生成验证码
    public static String yanZhengMa(){
        ArrayList<Character> list = new ArrayList<>();
        for(int i = 0;i<26;i++){
            list.add(((char)('a' + i)));
            list.add(((char)('A' + i)));
        }
        Random random = new Random();
        String result = "";
        for(int i = 0;i<4;i++){
            int arrRandom = random.nextInt(list.size());
            result += list.get(arrRandom);
        }
        int intRandom = random.nextInt(10);
        result += intRandom;
        char[] c = result.toCharArray();
        int cRandom = random.nextInt(c.length);
        char temp = c[c.length-1];
        c[c.length-1] = c[cRandom];
        c[cRandom] = temp;
        StringBuilder sb = new StringBuilder();
        sb.append(c);
        return sb.toString();
    }

    //验证用户名格式：只能为字母加数字的组合，不能为纯数字
    public static boolean checkUsername(String username){
        //用户名满足字母加数字的组合
        int lent = username.length();
        for (int j = 0; j < lent; j++) {
            char c = username.charAt(j);
            if(!((c<='z'&&c>='a')||(c<='Z'&&c>='A')||(c<='9'&&c>='0'))){
                return false;
            }
        }
        //用户名满足不能为纯数字
        //统计字母个数
        int count = 0;
        for (int i = 0; i < lent; i++) {
            char c = username.charAt(i);
            if((c<='z' && c>='a')||(c<='Z' && c>='A')){
                count++;
                //当出现字母时直接退出循环，提升效率
                break;
            }
        }
        return count>0;
    }
}
```
## StudentSystem类：登录成功后进入学生管理系统的核心功能，包括添加学生、删除学生、修改学生信息、查询学生信息等。

```java


import java.util.ArrayList;
import java.util.Scanner;

public class StudentSystem {

    static ArrayList<Student> list = new ArrayList<>();
    static{
        Student s1 = new Student("01","张三","23","q");
        Student s2 = new Student("02","李四","24","w");
        Student s3 = new Student("03","王五","25","e");
        list.add(s1);
        list.add(s2);
        list.add(s3);
    }


    private static final String ADD_STUDENT = "1";
    private static final String DELETE_STUDENT = "2";
    private static final String UPDATE_STUDENT = "3";
    private static final String QUERY_STUDENT = "4";
    private static final String EXIT = "5";

    public static void startStudentSystem() {
        loop: while (true) {
            System.out.println("--------欢迎来到学生管理系统--------");
            System.out.println("1：添加学生");
            System.out.println("2：删除学生");
            System.out.println("3：修改学生");
            System.out.println("4：查询学生");
            System.out.println("5：退出");
            System.out.println("请输入您的选择：");
            Scanner sc = new Scanner(System.in);
            String choose = sc.next();
            switch (choose) {
                case ADD_STUDENT -> addStudent(list);
                case DELETE_STUDENT -> deleteStudent(list);
                case UPDATE_STUDENT -> updateStudent(list);
                case QUERY_STUDENT -> queryStudent(list);
                case EXIT -> {
                    System.out.println("退出");
                    break loop;
//                    System.exit(0);
                }
                default -> System.out.println("请输入已有的选项");
            }
        }
    }

//添加学生
    public static void addStudent(ArrayList<Student> list){
        Scanner sc = new Scanner(System.in);
        Student s = new Student();
        System.out.println("添加学生的信息");
        System.out.println("请输入id");
        while (true) {
            String sid = sc.next();
            boolean flag = contains(list,sid);
            if(flag){
                System.out.println("id已存在，添加失败,请重新录入");
            }else{
                s.setId(sid);
                break;
            }
        }
        System.out.println("请输入姓名");
        String sname = sc.next();
        s.setName(sname);
        System.out.println("请输入年龄");
        while (true) {
            String sage = sc.next();
            boolean flag = isNum(sage);
            if(flag){
                s.setAge(sage);
                break;
            }else {
                System.out.println("年龄只能为数字，请重新输入年龄。");
            }
        }
        System.out.println("请输入家庭住址");
        String saddress = sc.next();
        s.setAddress(saddress);
        list.add(s);
        System.out.println("添加成功");
    }

//删除学生
    public static void deleteStudent(ArrayList<Student> list){
        System.out.println("请输入要删除的学生的id");
        for(int i = 0;i<list.size();i++){
            Scanner sc = new Scanner(System.in);
            String sid = sc.next();
            boolean flag = contains(list,sid);
            if(flag){
                list.remove(getIndex(list,sid));
                System.out.println("删除成功");
                break;
            }else{
                System.out.println("该用户不存在，删除失败。");
                return;
            }
        }
    }

//修改学生
    public static void updateStudent(ArrayList<Student> list){
        System.out.println("请输入需要修改的学生的id");
        Scanner sc = new Scanner(System.in);
        String sid = sc.next();
        boolean flag = contains(list,sid);
        int index = getIndex(list,sid);
        if(flag){
            System.out.println("请输入需要修改的信息");
            System.out.println("修改学生姓名");
            String sname = sc.next();
            list.get(index).setName(sname);
            System.out.println("修改学生年龄");
            while (true) {
                String sage = sc.next();
                boolean flag2 = isNum(sage);
                if(flag2){
                    list.get(index).setAge(sage);
                    break;
                }else {
                    System.out.println("年龄只能为数字，请重新输入年龄。");
                }
            }
            System.out.println("修改学生家庭住址");
            String saddress = sc.next();
            list.get(index).setAddress(saddress);
            System.out.println("修改完成");
        }else{
            System.out.println("该学生id不存在");
        }
    }

//查找学生
    public static void queryStudent(ArrayList<Student> list){
        System.out.println("查询学生");
        if(list.size() == 0){
            System.out.println("当前无学生信息，请添加后查询。");
            return;//结束方法
        }
        System.out.println("id\t姓名\t年龄\t家庭住址");
        for (int i = 0;i<list.size();i++){
            Student stu = list.get(i);
            String sid = stu.getId();
            String sname = stu.getName();
            String sage = stu.getAge();
            String saddress = stu.getAddress();
            System.out.println(sid+"\t"+sname+"\t"+sage+"\t"+saddress);
        }
    }

//对id进行判断
    public static boolean contains(ArrayList<Student> list,String id){
//        循环遍历集合得到每个学生对象
//        对id进行判断
//        存在为true,不存在为false
//        for (int i = 0;i<list.size(); i++) {
//            Student stu = list.get(i);
//            String sid = stu.getId();
//            if(sid.equals(id)){
//                return true;
//            }
//        }
//        return false;
        return getIndex(list,id) >= 0;
    }

//通过id获取索引
    public static int getIndex(ArrayList<Student> list,String id) {
        //通过id获取索引
        for (int i = 0; i < list.size(); i++) {
            if (id.equals(list.get(i).getId())) {
                return i;
            }
        }
        return -1;
    }

//判断输入年龄是否为数字
    public static boolean isNum(String age){
        for(int i = 0;i<age.length();i++){
            char c = age.charAt(i);
            if((c>='0')&&(c<='9')){
                return true;
            }
        }
        return false;
    }
}


```

## 学生的JavaBean类

```java
public class Student {
    private String id;
    private String name;
    private String age;
    private String address;

    public Student() {
    }

    public Student(String id, String name, String age, String address) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

```
## 用户的JavaBean类

```java
public class User {
    private String username;
    private String password;
    private String id;
    private String phoneNumber;

    public User() {
    }

    public User(String username, String password, String id, String phoneNumber) {
        this.username = username;
        this.password = password;
        this.id = id;
        this.phoneNumber = phoneNumber;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getPhoneNumber() {
        return phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}


```