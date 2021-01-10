### IO流

#### 一.基本数据流

InputStream/OutputStream/Writer/Reader

节点流：底层流，连接程序和操作系统  fileinputstream/byteArrayinputstream

操作流：对底层流的加强

#### 二 相对路径和绝对路径

带盘符的是绝对路径 D:

不带盘符的是相对路径 

由于不同操作系统的分隔符不同，所以在对路径进行操作时，我们做好采用以下处理

```java
F:/codeing/Files/dir/test/test.txt
采用"/” 反斜杠
    
而不是
    F:\\codeing\\Files\\dir\\test\\test.txt
```

#### 三 什么是File

file是一个对象，一个文件资源

#### 四  对文件夹和文件的操作

##### 4.1 获取文件路径/创建文件

```java
public class Test_01 {
    public static void main(String[] args) throws IOException {
        // 获取资源
        File file = new File("F:/codeing/Files/dir/test");
        // 创建文件夹，当文件路径又不存在的时候不创建（如dir文件夹不存在）
        boolean mkdir = file.mkdir();
        System.out.println(mkdir);

        File files = new File("F:/codeing/Files/dir/test");
        // 创建文件资源，当路径中又不存在的时候，一同创建（如dir文件不存在）
        boolean mkdirs = files.mkdirs();
        System.out.println(mkdirs);

        File text = new File("F:/codeing/Files/dir/test/test.txt");
        // 判断当前文件是否存在
        boolean exists = text.exists();
        System.out.println(exists);


        File text2 = new File("text.txt");
        System.out.println(text2.exists());

        File text3 = new File("text.txt");
        // 通过相对路劲创建文件
        text3.createNewFile();
        System.out.println(text3.exists());

        File text4 = new File("F:/codeing/Files/dir/test/text.txt");
        // 通过绝对路径创建文件
        text4.createNewFile();
        System.out.println(text4.exists());

        File absoluteFile = text4.getAbsoluteFile();
        System.out.println("absoluteFile:"+absoluteFile);
        System.out.println("getAbsolutePath"+text4.getAbsolutePath());
        System.out.println("getName:"+text4.getName());
        System.out.println("getParent"+text4.getParent());
        System.out.println("getParentFile"+text4.getParentFile());


    }
}
```



##### 4.2  list/listFiles 获取文件名称和文件对象

```java
public class file_02 {
    public static void main(String[] args) {
        File file = new File("F:/codeing/Files/dir/test");

   // 获取根对象     System.out.println("listRoots:"+file.listRoots());
        // 下级名称,直接获取文件名称  String
        String[] list = file.list();
        for (String s : list) {
            System.out.println(s);
        }

        //下级对象 File
        File[] files = file.listFiles();
        for (File file1 : files) {
            System.out.println(file1);
        }

    }
}
```

---------------

##### 4.3 使用面向对象获取文件夹大小

​    普通

```java
public class file_04 {
    private static Long size=0L;
    public static void main(String[] args) {
        File file = new File("F:/codeing/springcloud-day1-resttemplate");
        countSize(file);
        System.out.println(size);
    }

    private static void countSize(File file) {
        if (null==file||!file.exists()){
            return ;
        }else{
            if (file.isFile()){
                size+=file.length();
            }else{
                for (File fi : file.listFiles()) {
                    countSize(fi);
                }
            }
        }

    }
}
```

对象------ 这个方法让我明白this的用处，就是在一起可以区分，以及为成员属性构建属性之后的使用带来方便

```java
public class DirCount {
    private long len;
    private String path;
    private File src;
    private long filesize;
    private long dirsize;

    public long getFilesize() {
        return filesize;
    }

    public void setFilesize(long filesize) {
        this.filesize = filesize;
    }

    public long getDirsize() {
        return dirsize;
    }

    public void setDirsize(long dirsize) {
        this.dirsize = dirsize;
    }

    public DirCount(String path) {
        this.path = path;
        this.src = new File(path);
        countSize(src);
    }

    public long getLen() {
        return len;
    }

    public void setLen(long len) {
        this.len = len;
    }

    public String getPath() {
        return path;
    }

    public void setPath(String path) {
        this.path = path;
    }

    public File getSrc() {
        return src;
    }

    public void setSrc(File src) {
        this.src = src;
    }

    public static void main(String[] args) {
        DirCount dirCount = new DirCount("F:/codeing/springcloud-day1-resttemplate");

        System.out.println(dirCount.getLen());
        System.out.println(dirCount.getFilesize());
        System.out.println(dirCount.getDirsize());
    }

    private void countSize(File file) {
        if (null == file || !file.exists()) {
            return;
        } else {
            if (file.isFile()) {
                len += file.length();
                filesize++;
            } else {
                for (File fi : file.listFiles()) {
                    countSize(fi);
                    dirsize++;
                }
            }
        }

    }
}
```

##### 4.4 获取文件名称

```java
public class file_03 {
    public static void main(String[] args) {
        File file = new File("F:/codeing/springcloud-day1-resttemplate");
        printName(file, 0);
    }

    public static void printName(File src, int deep) {
        for (int i = 0; i < deep; i++) {
            System.out.print("-");
        }
        System.out.println(src.getName());
        if (null == src || !src.exists()) {
            return;
        } else if (src.isDirectory()) {
            for (File file : src.listFiles()) {
                printName(file, deep++);
            }
        }

    }
}
```

#### 五 字节输出输入流-文件的读取和输出

##### 5.1 读取 FileInputStream

```java
public class file_05 {
    public static void main(String[] args) {
        File file = new File("F:/codeing/Files/text.txt");
        InputStream is = null;
        try {
            is = new FileInputStream(file);
            int data1 = is.read();
            int data2 = is.read();
            int data3 = is.read();
            // 当字节流读取不到是 返回-1
            int data4 = is.read(); 
            System.out.println((char) data1);
            System.out.println((char) data2);
            System.out.println((char) data3);
            System.out.println(data4);
            // 只是通知系统释放资源而不能操作释放
            is.close();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

改造

```java
public class file_05 {
    public static void main(String[] args) {
        File file = new File("F:/codeing/Files/text.txt");
        InputStream is = null;
        try {
            is = new FileInputStream(file);
            int temp;
            while ((temp = is.read()) != -1) {
                System.out.println((char) temp);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
           try {
               if (null != is) {
                   is.close();
               }
           } catch (IOException e) {
               e.printStackTrace();
           }
        }

    }
}
```

##### 5.2 分段读取 添加数组作为缓冲区

```java
public class file_06 {
    public static void main(String[] args) {
        File file = new File("F:/codeing/Files/text.txt");
        InputStream is = null;
        try {
            is = new FileInputStream(file);
            //分段读取
            byte[] car = new byte[3];
            int len=-1;
            while ((len = is.read(car)) != -1) {
            // 这里的数组是作为数据的存储地，所以要把这个作为缓冲区的数组作为参数
                String s = new String(car, 0, len);
                System.out.println(s);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
           try {
               if (null != is) {
                   is.close();
               }
           } catch (IOException e) {
               e.printStackTrace();
           }
        }

    }
}

```

##### 5.3 文件输出 FileOutputStream

```java
public class file_07 {
    public static void main(String[] args) {
    // 输出到哪里去
        File file = new File("out.txt");
        OutputStream out = null;
        try {
            out = new FileOutputStream(file,false);//新增 true:追加 false：覆盖元数据
            String msg = "welcom to new world!\r\n";
            byte[] msgBytes = msg.getBytes();
            out.write(msgBytes, 0, msgBytes.length);
            out.flush();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (null != out) {
                    out.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}

```

##### 5.4 文件输入输出（拷贝）

```java
public class file_08 {
    public static void main(String[] args) {
        File filein = new File("p.png");
        File fileout = new File("pcopy.png");
        OutputStream out = null;
        InputStream is = null;
        try {
            is = new FileInputStream(filein);
            out = new FileOutputStream(fileout,true);//新增 true:追加 false：覆盖
            //分段读取
            byte[] flush = new byte[1024];
            int len=-1;
            while ((len = is.read(flush)) != -1) {
            // 获取数据的缓冲区flush
                out.write(flush, 0, len);
            }
            out.flush();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (null != out) {
                    out.close();
                }
                if (null != is) {
                    is.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

#### 六 字符输出输入流-文件的读取和输出

##### 6.1 读取-FileReader

```java
public class file_09 {
    public static void main(String[] args) {
        File file = new File("text.txt");
        FileReader reader = null;
        try {
            reader = new FileReader(file);
            //分段读取
            char[] flush = new char[1024];
            int len=-1;
            while ((len = reader.read(flush)) != -1) {
                String s = new String(flush, 0, len);
                System.out.println(s);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
           try {
               if (null != reader) {
                   reader.close();
               }
           } catch (IOException e) {
               e.printStackTrace();
           }
        }

    }
}
```

##### 6.2  输出-FileWriter

```java
public class file_10 {
    public static void main(String[] args) {
        File file = new File("out.txt");
        Writer out = null;
        try {
            out = new FileWriter(file, false);//新增 true:追加 false：覆盖
            String msg = "new world!\r\n";
            //1
            //char[] msgBytes = msg.toCharArray();
            //out.write(msgBytes, 0, msgBytes.length);

            //2
            //out.write(msg);

            //3
            out.append(msg).append("hhhhhhh");
            out.flush();

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (null != out) {
                    out.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

#### 七 文件操作的标准步骤

##### 7.1 数据源 

```
 File file = new File("out.txt")
```



##### 7.2 选择流

```
InputStream is = new FileInputStream(file)
```



##### 7.3 操作

```
 try {
            is = new FileInputStream(file);
            int temp;
            while ((temp = is.read()) != -1) {
                System.out.println((char) temp);
            }
        }
```



##### 7.4 释放资源

```
  is.close();
```

#### 八 字节数组流 ByteArrayInputStream/ByteArrayOutputStream

1.字节数组相当于另外一块内存，应为他是直接和内存连接在一起的，里面是一个空方法，只是代替了内存，这个字节数组可作为源数据，方便作为网络传输

2.这个流是直接和内存打交道的；所以这个流所占用的资源不用关闭，而是由系统自己来关闭

3.应为是内存，所以要注意大小

4.是和字节数组打交道byte[]src

##### ByteArrayInputStream

```java
public class file_11 {
    public static void main(String[] args) {
        //1.创建源
        byte[] src = "talk is cheap show me the code".getBytes();
        //2.选择流
        InputStream is = null;
        try {
            //将源和处理流整合在一起
            is = new ByteArrayInputStream(src);
            // 3. 操作
            byte[] flush = new byte[5];//缓冲容器
            int len = -1;//接收长度
            while ((len = is.read(flush)) != -1) {
                //字节数组-》字符串（解码）
               String s = new String(flush, 0, len);
                System.out.println(s);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

##### ByteArrayOutputStream

不需要指定目的地--》应为是操作内存，所以需要去内存获取数据，无法指定

内存不方便控制，让系统自己决定，自动开辟了缓冲区



```

public class file_13 {
    public static void main(String[] args) {
        //1.创建源---》内部维护
        byte[] desc =null;
        //2.选择流---》不关联源
        ByteArrayOutputStream baos = null;
        try {
            //将源和处理流整合在一起
            baos = new ByteArrayOutputStream();
            // 3. 操作---》写出内容
            String msg="show me the code";
            byte[]datas=msg.getBytes();
            baos.write(datas,0,datas.length);
            baos.flush();;
           //获取数据
            desc=baos.toByteArray();
            System.out.println(desc.length);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 九 IO综合对接流--图片的读取与输出

```java
public class file_14 {
    public static void main(String[] args) {
        byte[] bytes = fileToBytearray("p.png");
        System.out.println(bytes.length);
        byteArrayToFile(bytes,"pbyte.png");
    }

    // 图片到字节数组
    public static byte[] fileToBytearray(String filePath){
        //1.创建源与目的地
        File src = new File(filePath);
        byte[] desc =null;
        //2.选择流
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            //将源和处理流整合在一起
            is = new FileInputStream(src);
            baos = new ByteArrayOutputStream();
            // 3. 操作
            byte[] flush = new byte[1024*10];//缓冲容器
            int len = -1;//接收长度
            while ((len = is.read(flush)) != -1) {
                //写出到内存
                baos.write(flush,0,len);
            }
            baos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return baos.toByteArray();
    }
    // 字节数组写出到图片
    public static void byteArrayToFile(byte[] src,String path){
       //1.创建源
        File file = new File(path);
        //2.选择流
        InputStream is = null;
        OutputStream os = null;
        try {
            //输入流和内存中的数据整合
            is=new ByteArrayInputStream(src);
            //输出路径和输出流整合
            os=new FileOutputStream(file);
            // 分段读取
            byte[]flush=new byte[1024];//缓冲容器
            int len=-1;//接受长度
            while ((len=is.read(flush))!=-1){
                os.write(flush,0,len);
            }
            os.flush();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (null==os){
                    os.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if (null==is){
                    is.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 十 字节缓冲流

套接节点流

直接释放处理流，会自动寻找节点流进行释放

#####  BufferInputStream

```java
public class file_15 {
    public static void main(String[] args) {
        File file = new File("text.txt");
        InputStream is = null;
        BufferedInputStream bis=null;
        try {
            is = new FileInputStream(file);
            bis=new BufferedInputStream(is);
            //分段读取
            byte[] flush = new byte[1024];
            int len=-1;
            while ((len = bis.read(flush)) != -1) {
                String s = new String(flush, 0, len);
                System.out.println(s);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
           try {
               if (null != bis) {
                   bis.close();
               }
           } catch (IOException e) {
               e.printStackTrace();
           }
        }

    }
}

```

##### BufferOutputStream

```

```

##### BufferReader

```java
public class file_16 {
    public static void main(String[] args) {
        File filein = new File("text.txt");
        BufferedReader reader=null;
        try {
             reader = new BufferedReader(new FileReader(filein));
            //分段读取
            String line=null;
            int len=-1;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (null != reader) {
                    reader.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}

```

##### BufferWriter

```

```



#### 十一 转换流

##### InputStreamReader/OutputStreamWriter

 操作文本等比较方便

```java
public class convert {
    public static void main(String[] args) {
        //操作system.in/system.out
        // system.in 字节流-》InputstreamReadser 字符流----
        // 字符缓冲流
        try {
            BufferedReader irs = new BufferedReader(new InputStreamReader(System.in));
            BufferedWriter ors = new BufferedWriter(new OutputStreamWriter(System.out));
            // 循环获取键盘的输入输出
            String msg="";
            while (!msg.equals("exist")){
                msg=irs.readLine();
                ors.write(msg);
                ors.newLine();
                ors.flush();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

##### 操作网络流

```java
public class convert2 {
    public static void main(String[] args) {
      // 操作网络流，下载百度源码
        try(InputStreamReader is =new InputStreamReader(new URL("http://www.naidu.com").openStream(),"UTF-8") ;) {
            // 操作
            int temp;
            while ((temp=is.read())!=-1){
                System.out.println((char)temp);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
-------------------------------------------------------

升级

public class convert2 {
    public static void main(String[] args) {
      // 操作网络流，下载百度源码
        try(BufferedReader is =new BufferedReader(new InputStreamReader(new URL("http://www.naidu.com").openStream(),"UTF-8")) ;) {
            // 操作
            String temp;
            while ((temp=is.readLine())!=null){
                System.out.println(temp);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

---------------------------
FileOutputStream
public class convert2 {
    public static void main(String[] args) {
      // 操作网络流，下载百度源码
        try(BufferedReader is =new BufferedReader(new InputStreamReader(new URL("http://www.naidu.com").openStream(),"UTF-8")) ;
            BufferedWriter writer=new BufferedWriter(new OutputStreamWriter(new FileOutputStream("baidu.html"),"UTF-8"));
        ) {
            // 操作
            String temp;
            while ((temp=is.readLine())!=null){
//                System.out.println(temp);
                writer.write(temp);
                writer.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 十二 数据流

方便后期直接获取字符类型，而不用在经过处理获取

按照顺序写，按照顺序读，不然读取会出现问题

##### DataInputStream

##### DateOutputStream

```java
public class Data_test01 {
    public static void main(String[] args) throws IOException {
        /**
         * 数据流
         * 1.写出后读取
         * 2. 读取的顺序和写出的顺序一样
         */

        //写出
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
//        DataOutputStream dos = new DataOutputStream(baos);
        //加上 BufferedOutputStream
        DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(baos));
        // 操作数据类型
        dos.writeUTF("编码辛酸泪");
        dos.write(17);
        dos.writeChar('b');
        dos.writeBoolean(true);
        dos.flush();
        byte[] datas = baos.toByteArray();
        //读取
        ByteArrayInputStream bis = new ByteArrayInputStream(datas);
//        DataInputStream dis = new DataInputStream(bis);
        // 加上BufferedInputStream
        DataInputStream dis = new DataInputStream(new BufferedInputStream(bis));
        System.out.println(dis.readUTF());
        System.out.println(dis.read());
        System.out.println(dis.readChar());
        System.out.println(dis.readBoolean());
    }
}
```

#### 十三 对象流（序列化流）

内存---》字节数组

serializable   给虚拟机看的，实现这个接口，作为通行证，来实现序列化

##### ObjectInputStream

##### ObjectOutputStream

```java
/**
 * 序列化流 不是所有的对象都可以实现序列化--》实现了serializable接口就可以
 */
public class ObjectTest {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        //写出
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        //序列化
        ObjectOutputStream dos = new ObjectOutputStream(new BufferedOutputStream(baos));
        // 操作数据类型
        dos.writeUTF("编码辛酸泪");
        dos.write(17);
        dos.writeChar('b');
        dos.writeBoolean(true);
        Employee e = new Employee("赵云", 40);
        dos.writeObject(e);
        dos.flush();
        byte[] datas = baos.toByteArray();
        //读取
        ByteArrayInputStream bis = new ByteArrayInputStream(datas);
        // 反序列化
        ObjectInputStream dis = new ObjectInputStream(new BufferedInputStream(bis));
        String ss = dis.readUTF();
        int read = dis.read();
        char c = dis.readChar();
        boolean b = dis.readBoolean();
        Object employee = dis.readObject();
        if (employee instanceof Employee){
            System.out.println(employee);
        }

    }
}
class Employee implements Serializable{
    private transient  String name; //transient 此参数不需要序列化
    private double salary;

    public Employee() {
    }

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", salary=" + salary +
                '}';
    }
}
```

##### 文件的序列化与反序列化（持久化）

```java
/**
 * 序列化流 不是所有的对象都可以实现序列化--》实现了serializable接口就可以
 */
public class ObjectTest2 {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        //写出
        //序列化
        ObjectOutputStream dos = new ObjectOutputStream(new BufferedOutputStream(new FileOutputStream("out.txt")));
        // 操作数据类型
        dos.writeUTF("编码辛酸泪");
        dos.write(17);
        dos.writeChar('b');
        dos.writeBoolean(true);
        Employee e = new Employee("赵云", 40);
        dos.writeObject(e);
        dos.flush();
        //读取
        // 反序列化
        ObjectInputStream dis = new ObjectInputStream(new BufferedInputStream(new FileInputStream("out.txt")));
        String ss = dis.readUTF();
        int read = dis.read();
        char c = dis.readChar();
        boolean b = dis.readBoolean();
        Object employee = dis.readObject();
        if (employee instanceof Employee){
            System.out.println(employee);
        }

    }
}
```

#### 十四 打印流

##### PrintStream

```java
/**
 * @Classname Printstream 打印流
 * @Description TODO
 * @Date 2020/12/2 21:55
 * @Created by huangchuan
 */
public class Printstream {
    public static void main(String[] args) throws FileNotFoundException {
        // 打印流System.out
        PrintStream ps = System.out;
        ps.println("打印流");
        ps.println(true);

        ps = new PrintStream(new BufferedOutputStream(new FileOutputStream("print.txt", true)), true);
        ps.println("打印流");
        ps.println(true);

        // 重定向输出端
        System.setOut(ps);
        System.out.println("chgange");

        // 重定向回控制台
        System.setOut(new PrintStream(new BufferedOutputStream(new FileOutputStream(FileDescriptor.out)), true));
        System.out.println("backing");
    }
}
```

##### PrintWriter

```java
/**
 * @Classname PrintWriter 打印流
 * @Description TODO
 * @Date 2020/12/2 21:55
 * @Created by huangchuan
 */
public class PrintWriterTest {
    public static void main(String[] args) throws FileNotFoundException {

        PrintWriter pw = new PrintWriter(new BufferedOutputStream(new FileOutputStream("print.txt")), true);
        pw.println("打印流");
        pw.println(true);
        pw.close();
    }
}
```

#### 十五 IO文件分割

##### RandomAccessFile随机访问

---》分段获取，多线程下载

```java
/**
 *随机读取和写入流
 */
public class randomAccessfileTest {
    public static void main(String[] args) throws IOException {
        RandomAccessFile accessFile = new RandomAccessFile(new File("src/economic.txt"), "r");
        // 随机读取(从第二个位置开始读取）
        accessFile.seek(2);
        // 读取
        // 3.分段操作
        byte[] flush = new byte[1024];
        String len=null;
        while ((len=accessFile.readLine())!=null){
            System.out.println(new String(len.getBytes("ISO-8859-1"),"utf-8"));//需要重新转码才能正常显示
        }
        accessFile.close();
    }
}

```

```java
/**
 *司机读取和写入流
 */
public class randomAccessfileTest02 {
    public static void main(String[] args) throws IOException {
        // 分多少块
        File src = new File("src/1111.java");
        //总长度
        long len=src.length();
        // 每块大小
        int blocKSize=10;

        int size= (int) Math.ceil(len*1.0/blocKSize);
        System.out.println(size);
        // 其实位置和实际大小
        int beginPos=0;
        int actualSize= blocKSize>len? (int) len :blocKSize;
        for (int i = 0; i < size; i++) {
            beginPos=i*blocKSize;
            if (i==size-1){//最后一块
                actualSize= (int) len;
            }else{
                actualSize=blocKSize;
                len-=actualSize;//剩余量
            }
            System.out.println(i+"-->"+beginPos+"-->"+actualSize);
            split(i,beginPos,actualSize);
        }
    }

    /**
     * 指定i块起的起始位置和长度
     * @param i
     * @param beginPos
     * @param actualSize
     * @throws IOException
     */
    // 分开思想  有个起始位置,实际大小
    public static void split(int i,int beginPos,int actualSize)throws IOException{
        RandomAccessFile accessFile = new RandomAccessFile(new File("src/1111.java"), "r");
        // 随机读取(从第beginPos位置开始读取）
        accessFile.seek(beginPos);
        // 读取
        // 3.分段操作
        byte[] flush = new byte[1024];
        int len=-1;
        while ((len=accessFile.read())!=-1){
            //System.out.println(new String(flush,0,len,"utf-8"));//需要重新转码才能正常显示
            if (actualSize>len){ //获取本此读取的所有内容
                System.out.println(new String(flush,0,len));
                actualSize-=len;

            }else{
                System.out.println(new String(flush,0,actualSize));
                break;

            }
        }
        accessFile.close();
    }
}

```

