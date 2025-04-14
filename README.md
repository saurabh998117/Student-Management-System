//# Student-Management-System
//A Java-based console application to manage student records using file handling and object-oriented programming. Features include add, view, and search //functionalities with data persistence using serialization.
import java.io.*;
import java.util.*;

class Student implements Serializable {
    private int id;
    private String name;
    private double marks;

    public Student(int id, String name, double marks) {
        this.id = id;
        this.name = name;
        this.marks = marks;
    }

    public int getId() { return id; }
    public String getName() { return name; }
    public double getMarks() { return marks; }

    @Override
    public String toString() {
        return "ID: " + id + " | Name: " + name + " | Marks: " + marks;
    }
}

public class StudentManagementSystem {
    private static final String FILE_NAME = "students.dat";

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        List<Student> students = loadStudents();

        while (true) {
            System.out.println("\n--- Student Management System ---");
            System.out.println("1. Add Student\n2. View All Students\n3. Search by ID\n4. Exit");
            System.out.print("Enter choice: ");
            int choice = sc.nextInt();

            switch (choice) {
                case 1 -> {
                    System.out.print("Enter ID: ");
                    int id = sc.nextInt();
                    sc.nextLine(); // consume newline
                    System.out.print("Enter Name: ");
                    String name = sc.nextLine();
                    System.out.print("Enter Marks: ");
                    double marks = sc.nextDouble();
                    students.add(new Student(id, name, marks));
                    saveStudents(students);
                    System.out.println("Student added successfully.");
                }

                case 2 -> {
                    System.out.println("\n--- All Students ---");
                    students.forEach(System.out::println);
                }

                case 3 -> {
                    System.out.print("Enter ID to search: ");
                    int id = sc.nextInt();
                    boolean found = false;
                    for (Student s : students) {
                        if (s.getId() == id) {
                            System.out.println(s);
                            found = true;
                            break;
                        }
                    }
                    if (!found) System.out.println("Student not found.");
                }

                case 4 -> {
                    System.out.println("Exiting...");
                    return;
                }

                default -> System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private static List<Student> loadStudents() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) {
            return (List<Student>) ois.readObject();
        } catch (Exception e) {
            return new ArrayList<>();
        }
    }

    private static void saveStudents(List<Student> students) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(FILE_NAME))) {
            oos.writeObject(students);
        } catch (IOException e) {
            System.out.println("Error saving students: " + e.getMessage());
        }
    }
}

