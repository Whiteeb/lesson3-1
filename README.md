package ru.beliy;

import java.io.*;
import java.util.ArrayList;

// 1. Прочитать файл (около 50 байт) в байтовый массив и вывести этот массив в консоль;
public class StreamDemoApp {
    public static void main(String[] args) {
        byte[] buf = new byte[50];
        try (FileInputStream in = new FileInputStream("demo.txt")) {
            int count;
            while ((count = in.read(buf)) > 0) {
                for (int i = 0; i < count; i++) {
                    System.out.print((char) buf[i]);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
//Последовательно сшить 5 файлов в один (файлы примерно 100 байт). Может пригодиться следующая конструкция: ArrayList<InputStream> al = new ArrayList<>(); ... Enumeration<InputStream> e = Collections.enumeration(al);
    public static void CombiningFiles() throws FileNotFoundException {
        byte[] buf = new byte[100];
        ArrayList<InputStream> al = new ArrayList<>();
        ArrayList<Byte> assembly = new ArrayList<>();
        FileInputStream in1 = new FileInputStream("demo1.txt");
        FileInputStream in2 = new FileInputStream("demo2.txt");
        FileInputStream in3 = new FileInputStream("demo3.txt");
        FileInputStream in4 = new FileInputStream("demo4.txt");
        FileInputStream in5 = new FileInputStream("demo5.txt");
        al.add(in1);
        al.add(in2);
        al.add(in3);
        al.add(in4);
        al.add(in5);
        for (int i = 0; i < al.size(); i++) {
            try (InputStream in = al.get(i)) {
                int count;
                while ((count = in.read(buf)) > 0) {
                    for (i = 0; i < count; i++) {
                        assembly.add(buf[i]);
                    }
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        try (FileOutputStream out = new FileOutputStream("demoAll.txt")) {
            byte[] buf2 = new byte[assembly.size()];
            for (int i = 0; i < assembly.size(); i++) {
                buf2[i] = assembly.get(i);
            }
            out.write(buf2);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
