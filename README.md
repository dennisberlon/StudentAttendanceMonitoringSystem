#StudentAttendanceMonitoringSystem

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import javax.swing.JOptionPane;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;

public class StudentAttendanceMonitoringSystem1 {

    // ==========================================
    // STUDENT CLASS
    // ==========================================

    static class Student {

        String studentId;
        String name;
        String block;
        String course;
        String year;

        ArrayList<String> attendance = new ArrayList<>();

        Student(String studentId, String name, String block,
                String course, String year) {

            this.studentId = studentId;
            this.name = name;
            this.block = block;
            this.course = course;
            this.year = year;
        }

        // ==========================================
        // DISPLAY STUDENT INFORMATION
        // ==========================================

        String getStudentInfo() {

            return "-----------------------------\n"
                    + "Student ID : " + studentId + "\n"
                    + "Name       : " + name + "\n"
                    + "Block      : " + block + "\n"
                    + "Program    : " + course + "\n"
                    + "Year       : " + year + "\n"
                    + "-----------------------------";
        }

        // ==========================================
        // DISPLAY ATTENDANCE RECORDS
        // ==========================================

        String getAttendanceInfo() {

            int present = 0;
            int absent = 0;

            StringBuilder result = new StringBuilder();

            result.append("Attendance Record:\n\n");

            if (attendance.isEmpty()) {

                result.append("No attendance record yet.\n");

            } else {

                for (String record : attendance) {

                    result.append(record).append("\n");

                    if (record.contains("Present")) {
                        present++;
                    }

                    if (record.contains("Absent")) {
                        absent++;
                    }
                }
            }

            result.append("\nTotal Present: ").append(present);
            result.append("\nTotal Absent : ").append(absent);

            return result.toString();
        }
    }

    // ==========================================
    // FIND STUDENT BY ID
    // ==========================================

    static Student findStudent(
            ArrayList<Student> students,
            String id) {

        for (Student student : students) {

            if (student.studentId.equals(id)) {
                return student;
            }
        }

        return null;
    }

    // ==========================================
    // CHECK IF DATE ALREADY EXISTS
    // FOR THE SAME STUDENT ONLY
    // ==========================================

    static boolean dateAlreadyUsedByStudent(
            Student student,
            String date) {

        for (String record : student.attendance) {

            if (record.startsWith(date + " -")) {
                return true;
            }
        }

        return false;
    }

    // ==========================================
    // SORT STUDENTS ALPHABETICALLY
    // ==========================================

    static void sortStudentsAlphabetically(
            ArrayList<Student> students) {

        Collections.sort(students, new Comparator<Student>() {

            @Override
            public int compare(Student s1, Student s2) {

                return s1.name.compareToIgnoreCase(s2.name);
            }
        });
    }

    // ==========================================
    // MAIN METHOD
    // ==========================================

    public static void main(String[] args) {

        ArrayList<Student> students = new ArrayList<>();

        int choice;

        do {

            // ==========================================
            // MAIN MENU
            // ==========================================

            String menu =
                    "======================================\n"
                    + "      ATTENDANCE MONITORING SYSTEM\n"
                    + "======================================\n"
                    + "1. Add Student\n"
                    + "2. View Student Records\n"
                    + "3. Mark Attendance\n"
                    + "4. Exit\n"
                    + "======================================\n";

            String choiceInput = JOptionPane.showInputDialog(
                    null,
                    menu + "\nEnter your choice:",
                    "Student Attendance System",
                    JOptionPane.QUESTION_MESSAGE
            );

            // If user presses Cancel
            if (choiceInput == null) {
                choice = 4;
                continue;
            }

            // ==========================================
            // VALIDATE MENU INPUT
            // ==========================================

            try {

                choice = Integer.parseInt(choiceInput);

            } catch (NumberFormatException e) {

                JOptionPane.showMessageDialog(
                        null,
                        "ERROR: Please enter a number from 1 to 4.",
                        "Invalid Input",
                        JOptionPane.ERROR_MESSAGE
                );

                choice = 0;
                continue;
            }

            // ==========================================
            // MENU OPTIONS
            // ==========================================

            switch (choice) {

                // ======================================
                // 1. ADD STUDENT
                // ======================================

                case 1:

                    String id;

                    // ==================================
                    // STUDENT ID VALIDATION
                    // ==================================

                    while (true) {

                        id = JOptionPane.showInputDialog(
                                null,
                                "Enter Student ID\n\n"
                                + "Correct format: 2025-01-12345\n"
                                + "Only IDs beginning with 2025 are accepted.",
                                "Student ID",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (id == null) {
                            break;
                        }

                        id = id.trim();

                        // Only 2025 Student IDs are accepted
                        if (!id.matches("2025-\\d{2}-\\d{5}")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid Student ID.\n\n"
                                    + "Correct format: 2025-01-12345\n"
                                    + "Only Student IDs beginning with 2025 are accepted.",
                                    "Invalid Student ID",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        // ==================================
                        // CHECK DUPLICATE STUDENT ID
                        // ==================================

                        Student existingStudent =
                                findStudent(students, id);

                        if (existingStudent != null) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Student ID already exists!\n\n"
                                    + "This student is already recorded.\n\n"
                                    + existingStudent.getStudentInfo(),
                                    "Duplicate Student",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            id = null;
                            break;
                        }

                        break;
                    }

                    // Return to menu
                    if (id == null) {
                        break;
                    }

                    // ==================================
                    // ENTER STUDENT NAME
                    // ==================================

                    String name;

                    while (true) {

                        name = JOptionPane.showInputDialog(
                                null,
                                "Enter Student Name:\n\n"
                                + "Required format:\n"
                                + "SURNAME, GIVEN NAME [SECOND NAME...] MIDDLE INITIAL.\n\n"
                                + "Examples:\n"
                                + "DELA CRUZ, JUAN R.\n",
                                "Student Information",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (name == null) {
                            break;
                        }

                        // Remove extra spaces and convert to uppercase
                        name = name.trim().toUpperCase();

                        // ==================================
                        // NAME FORMAT VALIDATION
                        // ==================================

                        /*
                         * Accepted:
                         *
                         * DELA CRUZ, JUAN R.
                         *
                         * Format:
                         *
                         * SURNAME, GIVEN NAME [SECOND NAME...] MIDDLE INITIAL.
                         */

                        if (!name.matches(
                                "[A-Z]+, [A-Z]+(?: [A-Z]+)* [A-Z]\\.")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid Student Name format.\n\n"
                                    + "Please follow this format:\n"
                                    + "SURNAME, GIVEN NAME [SECOND NAME...] MIDDLE INITIAL.\n\n"
                                    + "Examples:\n"
                                    + "DELA CRUZ, JUAN R.\n",
                                    "Invalid Name",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        break;
                    }

                    if (name == null) {
                        break;
                    }

                    // ==================================
                    // ENTER BLOCK
                    // ==================================

                    String block;

                    while (true) {

                        block = JOptionPane.showInputDialog(
                                null,
                                "Enter Block:\n\n"
                                + "Allowed value: BLOCK A",
                                "Student Information",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (block == null) {
                            break;
                        }

                        block = block.trim().toUpperCase();

                        // Only BLOCK A is accepted
                        if (!block.equals("BLOCK A")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid Block.\n\n"
                                    + "Only BLOCK A is accepted.",
                                    "Invalid Block",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        break;
                    }

                    if (block == null) {
                        break;
                    }

                    // ==================================
                    // ENTER PROGRAM / COURSE
                    // ==================================

                    String course;

                    while (true) {

                        course = JOptionPane.showInputDialog(
                                null,
                                "Enter Program/Course:\n\n"
                                + "Allowed value: BSIT",
                                "Student Information",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (course == null) {
                            break;
                        }

                        course = course.trim().toUpperCase();

                        // Only BSIT is accepted
                        if (!course.equals("BSIT")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid Program/Course.\n\n"
                                    + "Only BSIT is accepted.",
                                    "Invalid Course",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        break;
                    }

                    if (course == null) {
                        break;
                    }

                    // ==================================
                    // ENTER YEAR
                    // ==================================

                    String year;

                    while (true) {

                        year = JOptionPane.showInputDialog(
                                null,
                                "Enter Year:\n\n"
                                + "Allowed value: 2ND YEAR",
                                "Student Information",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (year == null) {
                            break;
                        }

                        year = year.trim().toUpperCase();

                        // Only 2 YEAR is accepted
                        if (!year.equals("2ND YEAR")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid Year.\n\n"
                                    + "Only 2ND YEAR is accepted.",
                                    "Invalid Year",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        break;
                    }

                    if (year == null) {
                        break;
                    }

                    // ==================================
                    // CREATE STUDENT
                    // ==================================

                    Student student = new Student(
                            id,
                            name,
                            block,
                            course,
                            year
                    );

                    // Save student
                    students.add(student);

                    // Sort students alphabetically
                    sortStudentsAlphabetically(students);

                    JOptionPane.showMessageDialog(
                            null,
                            "Student information saved successfully!\n\n"
                            + student.getStudentInfo(),
                            "Success",
                            JOptionPane.INFORMATION_MESSAGE
                    );

                    // ==================================
                    // AUTOMATIC FIRST ATTENDANCE
                    // ==================================

                    String date;

                    while (true) {

                        date = JOptionPane.showInputDialog(
                                null,
                                "Enter date (MM/DD/YYYY):",
                                "First Attendance Record",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (date == null) {
                            break;
                        }

                        date = date.trim();

                        // ==================================
                        // DATE FORMAT VALIDATION
                        // ==================================

                        if (!date.matches("\\d{2}/\\d{2}/\\d{4}")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid date format.\n"
                                    + "Please use MM/DD/YYYY.",
                                    "Invalid Date",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        // ==================================
                        // CHECK DUPLICATE DATE
                        // FOR THIS STUDENT ONLY
                        // ==================================

                        if (dateAlreadyUsedByStudent(
                                student,
                                date)) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: This student already has "
                                    + "an attendance record for "
                                    + date + ".\n\n"
                                    + "Please enter a different date.",
                                    "Duplicate Date",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        break;
                    }

                    if (date == null) {
                        break;
                    }

                    // ==================================
                    // ATTENDANCE STATUS
                    // ==================================

                    while (true) {

                        String status = JOptionPane.showInputDialog(
                                null,
                                "Attendance:\n"
                                + "P = Present\n"
                                + "A = Absent\n\n"
                                + "Enter attendance status:",
                                "Attendance",
                                JOptionPane.QUESTION_MESSAGE
                        );

                        if (status == null) {
                            break;
                        }

                        status = status.trim().toUpperCase();

                        if (status.equals("P")) {

                            student.attendance.add(
                                    date + " - Present"
                            );

                            JOptionPane.showMessageDialog(
                                    null,
                                    "Attendance marked as PRESENT.",
                                    "Attendance Recorded",
                                    JOptionPane.INFORMATION_MESSAGE
                            );

                            break;

                        } else if (status.equals("A")) {

                            student.attendance.add(
                                    date + " - Absent"
                            );

                            JOptionPane.showMessageDialog(
                                    null,
                                    "Attendance marked as ABSENT.",
                                    "Attendance Recorded",
                                    JOptionPane.INFORMATION_MESSAGE
                            );

                            break;

                        } else {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Please enter P or A.",
                                    "Invalid Attendance",
                                    JOptionPane.ERROR_MESSAGE
                            );
                        }
                    }

                    break;

                // ======================================
                // 2. VIEW STUDENT RECORDS
                // ======================================

                case 2:

                    if (students.isEmpty()) {

                        JOptionPane.showMessageDialog(
                                null,
                                "No student records found.",
                                "Student Records",
                                JOptionPane.INFORMATION_MESSAGE
                        );

                    } else {

                        // Sort before displaying
                        sortStudentsAlphabetically(students);

                        StringBuilder allRecords =
                                new StringBuilder();

                        allRecords.append(
                                "===== ALL STUDENT RECORDS =====\n\n"
                        );

                        for (Student studentRecord : students) {

                            allRecords.append(
                                    studentRecord.getStudentInfo()
                            );

                            allRecords.append("\n\n");

                            allRecords.append(
                                    studentRecord.getAttendanceInfo()
                            );

                            allRecords.append(
                                    "\n\n======================================\n\n"
                            );
                        }

                        // ==================================
                        // CREATE JTextArea
                        // ==================================

                        JTextArea recordsArea = new JTextArea(
                                allRecords.toString(),
                                25,
                                60
                        );

                        recordsArea.setEditable(false);
                        recordsArea.setLineWrap(false);

                        // ==================================
                        // WRAP JTextArea IN JScrollPane
                        // ==================================

                        JScrollPane recordsScrollPane =
                                new JScrollPane(recordsArea);

                        // ==================================
                        // PASS JScrollPane TO JOptionPane
                        // ==================================

                        JOptionPane.showMessageDialog(
                                null,
                                recordsScrollPane,
                                "All Student Records",
                                JOptionPane.INFORMATION_MESSAGE
                        );
                    }

                    break;

                // ======================================
                // 3. MARK ATTENDANCE
                // ======================================

                case 3:

                    if (students.isEmpty()) {

                        JOptionPane.showMessageDialog(
                                null,
                                "No students available.",
                                "Mark Attendance",
                                JOptionPane.WARNING_MESSAGE
                        );

                        break;
                    }

                    // ==================================
                    // SORT STUDENTS ALPHABETICALLY
                    // ==================================

                    sortStudentsAlphabetically(students);

                    // ==================================
                    // DISPLAY STUDENTS
                    // ==================================

                    StringBuilder studentList =
                            new StringBuilder();

                    studentList.append(
                            "===== MARK ATTENDANCE =====\n\n"
                    );

                    for (int i = 0; i < students.size(); i++) {

                        studentList.append(
                                (i + 1)
                                + ". "
                                + students.get(i).name
                                + " ("
                                + students.get(i).studentId
                                + ")\n"
                        );
                    }

                    studentList.append(
                            "\nSelect student number:"
                    );

                    // ==================================
                    // CREATE JTextArea
                    // ==================================

                    JTextArea studentListArea = new JTextArea(
                            studentList.toString(),
                            15,
                            45
                    );

                    studentListArea.setEditable(false);
                    studentListArea.setLineWrap(false);

                    // ==================================
                    // WRAP JTextArea IN JScrollPane
                    // ==================================

                    JScrollPane studentListScrollPane =
                            new JScrollPane(studentListArea);

                    // ==================================
                    // PASS JScrollPane TO JOptionPane
                    // ==================================

                    String numberInput =
                            JOptionPane.showInputDialog(
                                    null,
                                    studentListScrollPane,
                                    "Select Student",
                                    JOptionPane.QUESTION_MESSAGE
                            );

                    if (numberInput == null) {
                        break;
                    }

                    int number;

                    try {

                        number = Integer.parseInt(
                                numberInput.trim()
                        );

                    } catch (NumberFormatException e) {

                        JOptionPane.showMessageDialog(
                                null,
                                "ERROR: Please enter a valid student number.",
                                "Invalid Input",
                                JOptionPane.ERROR_MESSAGE
                        );

                        break;
                    }

                    // ==================================
                    // CHECK STUDENT NUMBER
                    // ==================================

                    if (number < 1 ||
                            number > students.size()) {

                        JOptionPane.showMessageDialog(
                                null,
                                "ERROR: Invalid student number.",
                                "Invalid Student",
                                JOptionPane.ERROR_MESSAGE
                        );

                        break;
                    }

                    Student selectedStudent =
                            students.get(number - 1);

                    // ==================================
                    // ENTER ATTENDANCE DATE
                    // ==================================

                    String attendanceDate;

                    while (true) {

                        attendanceDate =
                                JOptionPane.showInputDialog(
                                        null,
                                        "Student: "
                                        + selectedStudent.name
                                        + "\n\n"
                                        + "Enter date (MM/DD/YYYY):",
                                        "Attendance Date",
                                        JOptionPane.QUESTION_MESSAGE
                                );

                        if (attendanceDate == null) {
                            break;
                        }

                        attendanceDate =
                                attendanceDate.trim();

                        // ==================================
                        // DATE FORMAT VALIDATION
                        // ==================================

                        if (!attendanceDate.matches(
                                "\\d{2}/\\d{2}/\\d{4}")) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Invalid date format.\n"
                                    + "Please use MM/DD/YYYY.",
                                    "Invalid Date",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        // ==================================
                        // CHECK DATE FOR SELECTED STUDENT
                        // ONLY
                        // ==================================

                        if (dateAlreadyUsedByStudent(
                                selectedStudent,
                                attendanceDate)) {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: "
                                    + selectedStudent.name
                                    + " already has an attendance "
                                    + "record for "
                                    + attendanceDate
                                    + ".\n\n"
                                    + "Please enter a different date.",
                                    "Duplicate Date",
                                    JOptionPane.ERROR_MESSAGE
                            );

                            continue;
                        }

                        break;
                    }

                    if (attendanceDate == null) {
                        break;
                    }

                    // ==================================
                    // ENTER ATTENDANCE STATUS
                    // ==================================

                    while (true) {

                        String attendanceStatus =
                                JOptionPane.showInputDialog(
                                        null,
                                        "Student: "
                                        + selectedStudent.name
                                        + "\n\n"
                                        + "P = Present\n"
                                        + "A = Absent\n\n"
                                        + "Enter attendance status:",
                                        "Attendance Status",
                                        JOptionPane.QUESTION_MESSAGE
                                );

                        if (attendanceStatus == null) {
                            break;
                        }

                        attendanceStatus =
                                attendanceStatus
                                        .trim()
                                        .toUpperCase();

                        if (attendanceStatus.equals("P")) {

                            selectedStudent.attendance.add(
                                    attendanceDate
                                    + " - Present"
                            );

                            JOptionPane.showMessageDialog(
                                    null,
                                    "Attendance marked as PRESENT.",
                                    "Attendance Recorded",
                                    JOptionPane.INFORMATION_MESSAGE
                            );

                            break;

                        } else if (
                                attendanceStatus.equals("A")) {

                            selectedStudent.attendance.add(
                                    attendanceDate
                                    + " - Absent"
                            );

                            JOptionPane.showMessageDialog(
                                    null,
                                    "Attendance marked as ABSENT.",
                                    "Attendance Recorded",
                                    JOptionPane.INFORMATION_MESSAGE
                            );

                            break;

                        } else {

                            JOptionPane.showMessageDialog(
                                    null,
                                    "ERROR: Please enter P or A.",
                                    "Invalid Attendance",
                                    JOptionPane.ERROR_MESSAGE
                            );
                        }
                    }

                    break;

                // ======================================
                // 4. EXIT
                // ======================================

                case 4:

                    JOptionPane.showMessageDialog(
                            null,
                            "Thank you for using the system!",
                            "Exit",
                            JOptionPane.INFORMATION_MESSAGE
                    );

                    break;

                // ======================================
                // INVALID CHOICE
                // ======================================

                default:

                    JOptionPane.showMessageDialog(
                            null,
                            "ERROR: Invalid choice.\n"
                            + "Please select 1, 2, 3, or 4.",
                            "Invalid Choice",
                            JOptionPane.ERROR_MESSAGE
                    );
            }

        } while (choice != 4);
    }
}
