# Software Testing Methodologies Lab — Java Programs

**Keshav Memorial Institute of Technology (KMIT)** · Department of Computer Science and Engineering  
**B.Tech IV Year, I Semester (KR23)** · Academic Year 2026–27

This README contains the experiment material from the uploaded STM lab manual, including **complete Java programs, compile/run commands, sample outputs, and important execution notes**.

> **Intentionally omitted, as requested:** Experiment 1 (Selenium IDE recording) and Experiment 7(a) (Dynamic Test Data Submission). All other code exercises are included. The code has been formatted and obvious compilation/extraction errors corrected while preserving the lab's logic and test data. Example outputs are from the manual or reflect the corrected code; environment-specific results may differ.

## Prerequisites

- **Java JDK 11, 17, or later** and the `javac`/`java` commands on your PATH. Check using `java -version` and `javac -version`.
- A desktop GUI environment for Swing and screenshot experiments: **2, 3, 4(a), 4(b), 7(c)**.
- **H2 JDBC** for Experiment 5, **SQLite JDBC** for Experiment 6, and **Apache POI** for Experiment 7(d).
- **Windows Calculator** for Experiment 10 (Windows only).
- Unless shown otherwise, run Java examples from the directory containing the source file. The **public class and filename must match**.
- The commands below use **Windows** syntax for the JAR classpath separator (`;`). Linux/macOS uses `:` instead.

## Contents

- [Experiment 2 — GUI Checkpoint for a Single Property](#exp-2)
- [Experiment 3 — GUI Checkpoint for a Single Object](#exp-3)
- [Experiment 4(a) — Bitmap Checkpoint for a Single Object](#exp-4a)
- [Experiment 4(b) — Bitmap Checkpoint for a Screen Area](#exp-4b)
- [Experiment 5 — Database Checkpoint — Default Check (Record Exists)](#exp-5)
- [Experiment 6 — Database Checkpoint — Custom Check (Validate Specific Value)](#exp-6)
- [Experiment 7(b) — Data Driven Test Through Flat Files (CSV/TXT)](#exp-7b)
- [Experiment 7(c) — Data Driven Test Through Front-End Grids](#exp-7c)
- [Experiment 7(d) — Data Driven Test Through Excel](#exp-7d)
- [Experiment 7(e) — Batch Testing Without Parameter Passing](#exp-7e)
- [Experiment 7(f) — Batch Testing With Parameter Passing](#exp-7f)
- [Experiment 8 — Data Driven Batch](#exp-8)
- [Experiment 9 — Silent Mode Test Execution Without Interruption](#exp-9)
- [Experiment 10 — Test Case for Calculator in a Windows Application](#exp-10)

---

<a id="exp-2"></a>

## Experiment 2 — GUI Checkpoint for a Single Property

### Java code — `Exp2_SinglePropertyCheckpoint.java`

```java
import javax.swing.JFrame;

public class Exp2_SinglePropertyCheckpoint {
    public static void main(String[] args) throws InterruptedException {
        JFrame frame = new JFrame("Google");
        frame.setSize(400, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);

        String expectedTitle = "Google";
        String actualTitle = frame.getTitle();

        System.out.println("Actual Title : " + actualTitle);
        System.out.println("Expected Title : " + expectedTitle);

        if (actualTitle.equals(expectedTitle)) {
            System.out.println("PASS: Title is correct");
        } else {
            System.out.println("FAIL: Title is incorrect");
        }

        Thread.sleep(2000);
        frame.dispose();
    }
}
```

### Compile and run

```sh
javac Exp2_SinglePropertyCheckpoint.java
java Exp2_SinglePropertyCheckpoint
```

### Expected output

```text
Actual Title : Google
Expected Title : Google
PASS: Title is correct
```

**Notes:** Corrected the PDF’s `new Frame(...)` to `new JFrame(...)` so the `JFrame` program compiles.

---

<a id="exp-3"></a>

## Experiment 3 — GUI Checkpoint for a Single Object

### Java code — `Exp3_SingleObjectCheckpoint.java`

```java
import javax.swing.*;

public class Exp3_SingleObjectCheckpoint {
    public static void main(String[] args) throws InterruptedException {
        JFrame frame = new JFrame("Single Object Checkpoint");
        JTextField searchBox = new JTextField(20);

        JPanel panel = new JPanel();
        panel.add(new JLabel("Search:"));
        panel.add(searchBox);
        frame.add(panel);
        frame.setSize(400, 150);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);

        Thread.sleep(500);
        boolean displayed = searchBox.isShowing();
        boolean enabled = searchBox.isEnabled();

        System.out.println("Displayed: " + displayed);
        System.out.println("Enabled : " + enabled);
        System.out.println(displayed && enabled
                ? "PASS: Search box is ready"
                : "FAIL: Search box is not ready");

        Thread.sleep(2000);
        frame.dispose();
    }
}
```

### Compile and run

```sh
javac Exp3_SingleObjectCheckpoint.java
java Exp3_SingleObjectCheckpoint
```

### Expected output

```text
Displayed: true
Enabled : true
PASS: Search box is ready
```

**Notes:** Corrected the PDF’s `new Frame(...)` to `new JFrame(...)` and restored the Swing import.

---

<a id="exp-4a"></a>

## Experiment 4(a) — Bitmap Checkpoint for a Single Object

### Java code — `NewTest.java`

```java
import javax.swing.*;
import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.File;

public class NewTest {
    public static void main(String[] args) throws Exception {
        JFrame frame = new JFrame("Bitmap Checkpoint Test");
        JTextField searchBox = new JTextField("Search here", 20);
        JPanel panel = new JPanel();
        panel.add(searchBox);
        frame.add(panel);
        frame.setSize(350, 120);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);

        Thread.sleep(1000);
        File folder = new File("Screenshots");
        if (!folder.exists()) folder.mkdir();

        BufferedImage currentImage = new BufferedImage(
                searchBox.getWidth(), searchBox.getHeight(),
                BufferedImage.TYPE_INT_RGB);
        Graphics2D g2 = currentImage.createGraphics();
        searchBox.paint(g2);
        g2.dispose();

        File currentFile = new File("Screenshots/current_searchbox.png");
        ImageIO.write(currentImage, "png", currentFile);
        System.out.println("Current screenshot saved:");
        System.out.println(currentFile.getAbsolutePath());

        File baselineFile = new File("Screenshots/baseline_searchbox.png");
        if (!baselineFile.exists()) {
            System.out.println("Baseline image not found.");
            System.out.println("Rename current_searchbox.png as baseline_searchbox.png");
        } else {
            boolean result = compareImages(baselineFile, currentFile);
            if (result) {
                System.out.println("BITMAP CHECKPOINT : TEST PASS");
                System.out.println("Images are identical.");
            } else {
                System.out.println("BITMAP CHECKPOINT : TEST FAIL");
                System.out.println("Images are different.");
            }
        }

        Thread.sleep(2000);
        frame.dispose();
    }

    public static boolean compareImages(File img1, File img2) throws Exception {
        BufferedImage image1 = ImageIO.read(img1);
        BufferedImage image2 = ImageIO.read(img2);

        if (image1.getWidth() != image2.getWidth()
                || image1.getHeight() != image2.getHeight()) return false;

        for (int x = 0; x < image1.getWidth(); x++) {
            for (int y = 0; y < image1.getHeight(); y++) {
                if (image1.getRGB(x, y) != image2.getRGB(x, y)) return false;
            }
        }
        return true;
    }
}
```

### Compile and run

```sh
javac NewTest.java
java NewTest
```

### Expected output

```text
Current screenshot saved:
.../Screenshots/current_searchbox.png
BITMAP CHECKPOINT : TEST PASS
Images are identical.
```

**Notes:** The **first run** only saves `current_searchbox.png`. Copy or rename it to `Screenshots/baseline_searchbox.png`, then run again to obtain the comparison result. Pixel-exact comparison can be affected by display scaling or UI theme.

---

<a id="exp-4b"></a>

## Experiment 4(b) — Bitmap Checkpoint for a Screen Area

### Java code — `NewTest.java`

```java
import javax.swing.*;
import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.File;

public class NewTest {
    public static void main(String[] args) throws Exception {
        JFrame frame = new JFrame("Bitmap Area Checkpoint");
        JPanel panel = new JPanel(new FlowLayout());
        panel.add(new JLabel("Google Search"));
        panel.add(new JTextField(25));
        panel.add(new JButton("Search"));
        frame.add(panel);
        frame.setSize(800, 600);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);

        Thread.sleep(1000);
        BufferedImage fullImage = new BufferedImage(
                frame.getWidth(), frame.getHeight(), BufferedImage.TYPE_INT_RGB);
        Graphics2D g2 = fullImage.createGraphics();
        frame.paint(g2);
        g2.dispose();

        File folder = new File("SeleniumScreenshots");
        if (!folder.exists()) folder.mkdirs();

        int width = Math.min(800, fullImage.getWidth());
        int height = Math.min(600, fullImage.getHeight());
        BufferedImage currentArea = fullImage.getSubimage(0, 0, width, height);

        File currentFile = new File("SeleniumScreenshots/current_google_area.png");
        ImageIO.write(currentArea, "png", currentFile);
        System.out.println("Current screenshot saved:");
        System.out.println(currentFile.getAbsolutePath());

        File baselineFile = new File("SeleniumScreenshots/baseline_google_area.png");
        if (!baselineFile.exists()) {
            System.out.println("Baseline image not found.");
            System.out.println("Rename current_google_area.png to baseline_google_area.png");
        } else {
            BufferedImage baseline = ImageIO.read(baselineFile);
            double similarity = compareImages(baseline, currentArea);
            System.out.printf("Image Similarity : %.2f%%%n", similarity);
            if (similarity >= 95)
                System.out.println("BITMAP CHECKPOINT : TEST PASS");
            else
                System.out.println("BITMAP CHECKPOINT : TEST FAIL");
        }

        Thread.sleep(2000);
        frame.dispose();
    }

    public static double compareImages(BufferedImage img1, BufferedImage img2) {
        if (img1.getWidth() != img2.getWidth()
                || img1.getHeight() != img2.getHeight()) return 0;

        long totalPixels = (long) img1.getWidth() * img1.getHeight();
        long samePixels = 0;
        for (int x = 0; x < img1.getWidth(); x++) {
            for (int y = 0; y < img1.getHeight(); y++) {
                if (img1.getRGB(x, y) == img2.getRGB(x, y)) samePixels++;
            }
        }
        return ((double) samePixels / totalPixels) * 100;
    }
}
```

### Compile and run

```sh
javac NewTest.java
java NewTest
```

### Expected output

```text
Current screenshot saved:
.../SeleniumScreenshots/current_google_area.png
Image Similarity : 100.00%
BITMAP CHECKPOINT : TEST PASS
```

**Notes:** The **first run** only saves the current image. Copy it to `SeleniumScreenshots/baseline_google_area.png`, then run again. A similarity of **95% or more** passes. The sample captures the frame area (800 × 600), matching the manual. **4(a) and 4(b) both use `NewTest`**: run them in separate directories or replace `NewTest.java` between experiments.

---

<a id="exp-5"></a>

## Experiment 5 — Database Checkpoint — Default Check (Record Exists)

Requires the H2 JDBC JAR, shown as `h2-2.4.240.jar` in the manual. Put it in the directory containing `NewTest.java`.

### Java code — `NewTest.java`

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class NewTest {
    public static void main(String[] args) {
        try {
            Connection con = DriverManager.getConnection(
                    "jdbc:h2:./StudentDB", "sa", "");
            Statement stmt = con.createStatement();

            stmt.execute("CREATE TABLE IF NOT EXISTS STUDENT (" +
                    "ID INT PRIMARY KEY, " +
                    "NAME VARCHAR(50), " +
                    "EMAIL VARCHAR(100))");
            stmt.execute("DELETE FROM STUDENT");
            stmt.executeUpdate("INSERT INTO STUDENT VALUES " +
                    "(1, 'Ajay', 'ajay@gmail.com')");

            ResultSet rs = stmt.executeQuery(
                    "SELECT * FROM STUDENT WHERE ID = 1");
            if (rs.next()) {
                System.out.println("Record Exists");
                System.out.println("ID : " + rs.getInt("ID"));
                System.out.println("Name : " + rs.getString("NAME"));
                System.out.println("Email : " + rs.getString("EMAIL"));
                System.out.println("TEST PASSED");
            } else {
                System.out.println("Record Not Found");
                System.out.println("TEST FAILED");
            }

            rs.close();
            stmt.close();
            con.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Compile and run

```bat
javac -cp h2-2.4.240.jar NewTest.java
java -cp ".;h2-2.4.240.jar" NewTest
```

### Expected output

```text
Record Exists
ID : 1
Name : Ajay
Email : ajay@gmail.com
TEST PASSED
```

**Notes:** Creates or reuses local H2 database files named `StudentDB`, resets the `STUDENT` table, inserts Ajay, and checks whether ID 1 exists. **This code deletes existing rows in the STUDENT table.** Run in a disposable lab database.

---

<a id="exp-6"></a>

## Experiment 6 — Database Checkpoint — Custom Check (Validate Specific Value)

Requires the SQLite JDBC JAR, shown as `sqlite-jdbc-3.34.0.jar` in the manual. Put it next to the source file.

### Java code — `DatabaseCheckpointCustomCheck.java`

```java
import java.sql.*;

public class DatabaseCheckpointCustomCheck {
    static final String DB_URL = "jdbc:sqlite:student.db";

    public static void setupDatabase() throws SQLException {
        try (Connection conn = DriverManager.getConnection(DB_URL);
             Statement stmt = conn.createStatement()) {
            stmt.execute("CREATE TABLE IF NOT EXISTS students (" +
                    "id INTEGER PRIMARY KEY, name TEXT, marks INTEGER)");
            stmt.execute("DELETE FROM students");
            stmt.execute("INSERT INTO students VALUES (1, 'Amit', 78)");
            stmt.execute("INSERT INTO students VALUES (2, 'Neha', 45)");
            stmt.execute("INSERT INTO students VALUES (3, 'Ravi', 92)");
        }
    }

    public static boolean customCheckpoint(int studentId, String expectedStatus)
            throws SQLException {
        try (Connection conn = DriverManager.getConnection(DB_URL);
             PreparedStatement ps = conn.prepareStatement(
                     "SELECT marks FROM students WHERE id = ?")) {
            ps.setInt(1, studentId);
            ResultSet rs = ps.executeQuery();

            if (!rs.next()) {
                System.out.println("FAIL: No record found for id " + studentId);
                return false;
            }

            int marks = rs.getInt("marks");
            String actualStatus = (marks >= 50) ? "Pass" : "Fail";
            if (actualStatus.equals(expectedStatus)) {
                System.out.println("PASS: Student " + studentId +
                        " status = " + actualStatus +
                        " (Custom check matched)");
                return true;
            } else {
                System.out.println("FAIL: Student " + studentId +
                        " expected " + expectedStatus +
                        ", got " + actualStatus);
                return false;
            }
        }
    }

    public static void main(String[] args) throws Exception {
        setupDatabase();
        customCheckpoint(1, "Pass");
        customCheckpoint(2, "Fail");
        customCheckpoint(3, "Pass");
    }
}
```

### Compile and run

```bat
javac -cp sqlite-jdbc-3.34.0.jar DatabaseCheckpointCustomCheck.java
java -cp ".;sqlite-jdbc-3.34.0.jar" DatabaseCheckpointCustomCheck
```

### Expected output

```text
PASS: Student 1 status = Pass (Custom check matched)
PASS: Student 2 status = Fail (Custom check matched)
PASS: Student 3 status = Pass (Custom check matched)
```

**Notes:** Corrected the broken quotation marks in the PDF’s `customCheckpoint(…, "Pass"/"Fail")` calls. Marks **50 and above = Pass**; below 50 = Fail. `PreparedStatement` binds the student ID with `setInt(1, studentId)`. **The setup deletes existing rows from the local students table**; use a lab database.

---

<a id="exp-7b"></a>

## Experiment 7(b) — Data Driven Test Through Flat Files (CSV/TXT)

### Java code — `FlatFileTest.java`

```java
import java.io.*;

public class FlatFileTest {
    public static double calculateDiscount(double price, String category) {
        switch (category) {
            case "electronics": return price * 0.9;
            case "clothing": return price * 0.8;
            default: return price;
        }
    }

    public static void createFlatFile() throws IOException {
        try (PrintWriter pw = new PrintWriter(new FileWriter("testdata.txt"))) {
            pw.println("500,electronics");
            pw.println("300,clothing");
            pw.println("150,grocery");
        }
    }

    public static void flatFileTest() throws IOException {
        System.out.println("---- Data Driven Test via Flat File ----");
        try (BufferedReader br = new BufferedReader(
                new FileReader("testdata.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                double price = Double.parseDouble(parts[0]);
                String category = parts[1];
                double result = calculateDiscount(price, category);
                System.out.printf(
                        "price=%.1f, category=%s -> discounted=%.2f%n",
                        price, category, result);
            }
        }
    }

    public static void main(String[] args) throws IOException {
        createFlatFile();
        flatFileTest();
    }
}
```

### Compile and run

```sh
javac FlatFileTest.java
java FlatFileTest
```

### Expected output

```text
---- Data Driven Test via Flat File ----
price=500.0, category=electronics -> discounted=450.00
price=300.0, category=clothing -> discounted=240.00
price=150.0, category=grocery -> discounted=150.00
```

**Notes:** Creates and overwrites `testdata.txt`, then reads each row using `BufferedReader`, `readLine()`, `split(",")`, and `Double.parseDouble()`. Discounts: electronics 10%, clothing 20%, grocery 0%.

---

<a id="exp-7c"></a>

## Experiment 7(c) — Data Driven Test Through Front-End Grids

### Java code — `FrontEndGridTest.java`

```java
import javax.swing.*;
import javax.swing.table.DefaultTableModel;

public class FrontEndGridTest {
    public static double calculateDiscount(double price, String category) {
        switch (category) {
            case "electronics": return price * 0.9;
            case "clothing": return price * 0.8;
            default: return price;
        }
    }

    public static void main(String[] args) {
        JFrame frame = new JFrame("Front End Grid - Data Driven Test");
        String[] columns = {"Price", "Category", "Result"};
        DefaultTableModel model = new DefaultTableModel(columns, 0);

        Object[][] gridData = {
                {500.0, "electronics"},
                {300.0, "clothing"},
                {150.0, "grocery"}
        };

        for (Object[] row : gridData) {
            double price = (double) row[0];
            String category = (String) row[1];
            double result = calculateDiscount(price, category);
            model.addRow(new Object[]{
                    price, category, String.format("%.2f", result)
            });
        }

        JTable table = new JTable(model);
        frame.add(new JScrollPane(table));
        frame.setSize(400, 200);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

### Compile and run

```sh
javac FrontEndGridTest.java
java FrontEndGridTest
```

### Expected output

```text
Price    Category      Result
500.0    electronics   450.00
300.0    clothing      240.00
150.0    grocery       150.00
```

**Notes:** Displays the result in a **Swing JTable**, not the console. `Object[][]` stores input data; `DefaultTableModel.addRow()` appends calculated rows.

---

<a id="exp-7d"></a>

## Experiment 7(d) — Data Driven Test Through Excel

Requires **Apache POI OOXML** (manual version: `5.2.5`) and its transitive dependencies. Recommended: create a Maven project and add the dependency below. Copy `ExcelDrivenTest.java` into `src/main/java/`.

### Java code — `ExcelDrivenTest.java`

```java
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import java.io.*;

public class ExcelDrivenTest {
    public static double calculateDiscount(double price, String category) {
        switch (category) {
            case "electronics": return price * 0.9;
            case "clothing": return price * 0.8;
            default: return price;
        }
    }

    public static void createExcelFile() throws IOException {
        Workbook wb = new XSSFWorkbook();
        Sheet sheet = wb.createSheet("TestData");
        Object[][] data = {
                {"Price", "Category"},
                {500.0, "electronics"},
                {300.0, "clothing"},
                {150.0, "grocery"}
        };

        for (int r = 0; r < data.length; r++) {
            Row row = sheet.createRow(r);
            for (int c = 0; c < data[r].length; c++) {
                Cell cell = row.createCell(c);
                if (data[r][c] instanceof Double)
                    cell.setCellValue((Double) data[r][c]);
                else
                    cell.setCellValue((String) data[r][c]);
            }
        }

        try (FileOutputStream fos = new FileOutputStream("testdata.xlsx")) {
            wb.write(fos);
        }
        wb.close();
    }

    public static void excelDrivenTest() throws IOException {
        System.out.println("---- Data Driven Test via Excel ----");
        try (FileInputStream fis = new FileInputStream("testdata.xlsx");
             Workbook wb = new XSSFWorkbook(fis)) {
            Sheet sheet = wb.getSheetAt(0);
            for (Row row : sheet) {
                if (row.getRowNum() == 0) continue;
                double price = row.getCell(0).getNumericCellValue();
                String category = row.getCell(1).getStringCellValue();
                double result = calculateDiscount(price, category);
                System.out.printf(
                        "price=%.1f, category=%s -> discounted=%.2f%n",
                        price, category, result);
            }
        }
    }

    public static void main(String[] args) throws IOException {
        createExcelFile();
        excelDrivenTest();
    }
}
```

### Compile and run

Add to the `<dependencies>` section of `pom.xml`:

```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>5.2.5</version>
</dependency>
```

From the Maven project directory:

```sh
mvn compile
mvn org.codehaus.mojo:exec-maven-plugin:3.5.0:java -Dexec.mainClass=ExcelDrivenTest
```

If using manually downloaded JARs instead, put **all** required POI and transitive JARs in `lib/`, then on Windows run:

```bat
javac -cp "lib/*" ExcelDrivenTest.java
java -cp ".;lib/*" ExcelDrivenTest
```

### Expected output

```text
---- Data Driven Test via Excel ----
price=500.0, category=electronics -> discounted=450.00
price=300.0, category=clothing -> discounted=240.00
price=150.0, category=grocery -> discounted=150.00
```

**Notes:** Creates (overwrites) `testdata.xlsx` using `XSSFWorkbook`, then reads every data row (skipping the header). Maven resolves required transitive libraries more reliably than a partial hard-coded JAR list.

---

<a id="exp-7e"></a>

## Experiment 7(e) — Batch Testing Without Parameter Passing

### Java code — `BatchNoParams.java`

```java
public class BatchNoParams {
    public static void testAddition() {
        assert 2 + 3 == 5;
        System.out.println("test_addition: PASS");
    }

    public static void testSubtraction() {
        assert 5 - 2 == 3;
        System.out.println("test_subtraction: PASS");
    }

    public static void main(String[] args) {
        System.out.println("---- Batch Testing (No Parameters) ----");
        testAddition();
        testSubtraction();
    }
}
```

### Compile and run

```sh
javac BatchNoParams.java
java -ea BatchNoParams
```

### Expected output

```text
---- Batch Testing (No Parameters) ----
test_addition: PASS
test_subtraction: PASS
```

**Notes:** Use **`-ea`** to enable Java assertions. Without it, the `assert` statements are ignored, so the PASS messages alone would not prove the calculations were checked.

---

<a id="exp-7f"></a>

## Experiment 7(f) — Batch Testing With Parameter Passing

### Java code — `BatchWithParams.java`

```java
public class BatchWithParams {
    static void testOperation(int a, int b, char op, int expected) {
        int result = 0;
        switch (op) {
            case '+': result = a + b; break;
            case '-': result = a - b; break;
            case '*': result = a * b; break;
        }

        String status = (result == expected) ? "PASS" : "FAIL";
        System.out.printf(
                "Test: %d %c %d = %d (expected %d) -> %s%n",
                a, op, b, result, expected, status);
    }

    public static void main(String[] args) {
        int[][] testCases = {
                {2, 3, '+', 5},
                {10, 4, '-', 6},
                {3, 3, '*', 9},
                {5, 5, '-', 1}
        };

        System.out.println("---- Batch Testing (With Parameters) ----");
        for (int[] tc : testCases) {
            testOperation(tc[0], tc[1], (char) tc[2], tc[3]);
        }
    }
}
```

### Compile and run

```sh
javac BatchWithParams.java
java BatchWithParams
```

### Expected output

```text
---- Batch Testing (With Parameters) ----
Test: 2 + 3 = 5 (expected 5) -> PASS
Test: 10 - 4 = 6 (expected 6) -> PASS
Test: 3 * 3 = 9 (expected 9) -> PASS
Test: 5 - 5 = 0 (expected 1) -> FAIL
```

**Notes:** The **last test fails intentionally**, because `5 - 5` equals 0, but the expected value is 1. The `char` operator is stored as its integer character code in `int[][]`, then cast back using `(char) tc[2]`.

---

<a id="exp-8"></a>

## Experiment 8 — Data Driven Batch

### Java code — `DataDrivenBatch.java`

```java
import java.io.*;

public class DataDrivenBatch {
    public static double calculateDiscount(double price, String category) {
        switch (category) {
            case "electronics": return price * 0.9;
            case "clothing": return price * 0.8;
            default: return price;
        }
    }

    public static void createCsvFile() throws IOException {
        try (PrintWriter pw = new PrintWriter(new FileWriter("batch_data.csv"))) {
            pw.println("Price,Category,ExpectedResult");
            pw.println("500,electronics,450");
            pw.println("300,clothing,240");
            pw.println("150,grocery,150");
            pw.println("1000,electronics,900");
        }
    }

    public static void dataDrivenBatch() throws IOException {
        System.out.println("---- Data Driven Batch Test ----");
        int passCount = 0, failCount = 0;

        try (BufferedReader br = new BufferedReader(
                new FileReader("batch_data.csv"))) {
            String line = br.readLine(); // Skip CSV header.
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                double price = Double.parseDouble(parts[0]);
                String category = parts[1];
                double expected = Double.parseDouble(parts[2]);
                double actual = calculateDiscount(price, category);

                if (actual == expected) {
                    System.out.printf(
                            "PASS: %s, price=%.1f -> %.1f%n",
                            category, price, actual);
                    passCount++;
                } else {
                    System.out.printf(
                            "FAIL: %s, price=%.1f -> got %.1f, expected %.1f%n",
                            category, price, actual, expected);
                    failCount++;
                }
            }
        }

        System.out.println("\nBatch Summary: " + passCount +
                " Passed, " + failCount + " Failed");
    }

    public static void main(String[] args) throws IOException {
        createCsvFile();
        dataDrivenBatch();
    }
}
```

### Compile and run

```sh
javac DataDrivenBatch.java
java DataDrivenBatch
```

### Expected output

```text
---- Data Driven Batch Test ----
PASS: electronics, price=500.0 -> 450.0
PASS: clothing, price=300.0 -> 240.0
PASS: grocery, price=150.0 -> 150.0
PASS: electronics, price=1000.0 -> 900.0

Batch Summary: 4 Passed, 0 Failed
```

**Notes:** Restored the missing `import java.io.*;` from the PDF. The code creates and overwrites `batch_data.csv` and checks four rows. The exact `double` comparison matches the simple input values in this manual.

---

<a id="exp-9"></a>

## Experiment 9 — Silent Mode Test Execution Without Interruption

### Java code — `SilentModeTest.java`

```java
import java.util.logging.*;
import java.io.IOException;

public class SilentModeTest {
    static Logger logger = Logger.getLogger("SilentTestLogger");

    static void setupLogger() throws IOException {
        logger.setUseParentHandlers(false);
        FileHandler fh = new FileHandler("silent_test_log.txt");
        fh.setFormatter(new SimpleFormatter());
        logger.addHandler(fh);
    }

    static void testCase(String name, Object actual, Object expected) {
        try {
            if (actual.equals(expected)) {
                logger.info(name + ": PASS (actual=" + actual +
                        ", expected=" + expected + ")");
            } else {
                logger.severe(name + ": FAIL (actual=" + actual +
                        ", expected=" + expected + ")");
            }
        } catch (Exception e) {
            logger.severe(name + ": ERROR - " + e.getMessage());
        }
    }

    public static void main(String[] args) throws IOException {
        setupLogger();
        testCase("Addition Test", 2 + 2, 4);
        testCase("Subtraction Test", 10 - 5, 5);
        testCase("Multiplication Test", 3 * 3, 10);
        testCase("String Test", "abc".toUpperCase(), "ABC");
        System.out.println("Batch execution completed silently. " +
                "Check silent_test_log.txt for results.");
    }
}
```

### Compile and run

```bat
javac SilentModeTest.java
java SilentModeTest
type silent_test_log.txt
```

### Expected output

```text
Terminal:
Batch execution completed silently. Check silent_test_log.txt for results.

Log file (message portion):
INFO: Addition Test: PASS (actual=4, expected=4)
INFO: Subtraction Test: PASS (actual=5, expected=5)
SEVERE: Multiplication Test: FAIL (actual=9, expected=10)
INFO: String Test: PASS (actual=ABC, expected=ABC)
```

**Notes:** The multiplication test **fails intentionally**: 3 × 3 = 9, but the expected value is 10. `SimpleFormatter` adds timestamps and other metadata to the actual log, so the shown lines are only the message portions. This program logs results quietly; it does **not** launch a headless browser.

---

<a id="exp-10"></a>

## Experiment 10 — Test Case for Calculator in a Windows Application

### Java code — `CalculatorTest.java`

```java
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.image.BufferedImage;
import javax.imageio.ImageIO;
import java.io.File;

public class CalculatorTest {
    public static void main(String[] args) throws Exception {
        Runtime.getRuntime().exec("calc.exe");
        Thread.sleep(2000);

        Robot robot = new Robot();
        robot.setAutoDelay(200);
        typeKey(robot, KeyEvent.VK_7);
        typeKey(robot, KeyEvent.VK_ADD);
        typeKey(robot, KeyEvent.VK_5);
        typeKey(robot, KeyEvent.VK_ENTER);
        Thread.sleep(1000);

        Rectangle screenRect = new Rectangle(
                Toolkit.getDefaultToolkit().getScreenSize());
        BufferedImage capture = robot.createScreenCapture(screenRect);
        ImageIO.write(capture, "png", new File("calculator_result.png"));

        System.out.println("Test executed: 7 + 5 =");
        System.out.println("Screenshot saved as calculator_result.png -- " +
                "compare visually to confirm result is 12");
    }

    static void typeKey(Robot robot, int keyCode) {
        robot.keyPress(keyCode);
        robot.keyRelease(keyCode);
    }
}
```

### Compile and run

```sh
javac CalculatorTest.java
java CalculatorTest
```

### Expected output

```text
Test executed: 7 + 5 =
Screenshot saved as calculator_result.png -- compare visually to confirm result is 12
```

**Notes:** **Windows only.** Requires a visible desktop, an unlocked session, and Calculator in the foreground. Uses Enter (`VK_ENTER`) instead of the manual’s `VK_EQUALS` for a more reliable calculation trigger. The program **does not automatically assert that the result is 12**; inspect `calculator_result.png`. The PDF’s example screenshot shows 12. If Calculator launches slowly, increase the initial sleep.

---

## Important notes

1. **Two experiments reuse the class name `NewTest`** (4(a), 4(b), and also 5). Save each program to `NewTest.java` in its own directory, or replace the file before compiling the next experiment.
2. **Initial image-baseline setup:** 4(a) and 4(b) must create a baseline image before a successful image comparison can be reported.
3. **Intentional FAIL results:** the last test in 7(f) and the multiplication test in 9 are intentionally incorrect expected values; do not change them just to make every test pass.
4. **Data-file writes:** 5 and 6 reset their own database tables, 7(b) creates/overwrites `testdata.txt`, 7(d) creates/overwrites `testdata.xlsx`, and 8 creates/overwrites `batch_data.csv`. Use a disposable lab directory/database.
5. **PDF corrections:** 2/3 use `JFrame` instead of `Frame`; 6 has corrected status string arguments; 8 adds the missing `java.io` import; 10 uses Enter to trigger the calculator and its output describes what is actually printed. No Selenium IDE experiment (1) or dynamic data experiment (7a) is included.

*Reference: uploaded Software Testing Methodologies Lab manual (`r.pdf`, experiment pages 14–37).* 
