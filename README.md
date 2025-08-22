import java.sql.*;
import java.util.Scanner;

public class StudentManagement {
    static final String DB_URL = "jdbc:mysql://localhost:3306/studentdb";
    static final String USER = "root";     // change with your MySQL username
    static final String PASS = "password"; // change with your MySQL password

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        try (Connection conn = DriverManager.getConnection(DB_URL, USER, PASS)) {
            System.out.println("Connected to Database");

            while (true) {
                System.out.println("\n===== Student Management System =====");
                System.out.println("1. Add Student");
                System.out.println("2. View Students");
                System.out.println("3. Exit");
                System.out.print("Enter choice: ");
                int choice = sc.nextInt();

                if (choice == 1) {
                    System.out.print("Enter ID: ");
                    int id = sc.nextInt();
                    System.out.print("Enter Name: ");
                    String name = sc.next();
                    System.out.print("Enter Age: ");
                    int age = sc.nextInt();

                    String sql = "INSERT INTO students (id, name, age) VALUES (?, ?, ?)";
                    PreparedStatement pstmt = conn.prepareStatement(sql);
                    pstmt.setInt(1, id);
                    pstmt.setString(2, name);
                    pstmt.setInt(3, age);
                    pstmt.executeUpdate();
                    System.out.println("Student Added!");
                } else if (choice == 2) {
                    Statement stmt = conn.createStatement();
                    ResultSet rs = stmt.executeQuery("SELECT * FROM students");
                    while (rs.next()) {
                        System.out.println(rs.getInt("id") + " | " + rs.getString("name") + " | " + rs.getInt("age"));
                    }
                } else {
                    break;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        sc.close();
    }
}
