# DataChecker
This program takes the average of each line and accounts for potential exceptions.

import java.util.InputMismatchException;
import java.util.Scanner;
import java.io.*;

public class DataChecker {
    public static void main(String [] arg) throws IOException {
        Scanner inFile = new Scanner(new File("numbers4.2.txt"));
        double avg;
        int lineNum = 0, validLines = 0, corruptLines = 0;
        while (inFile.hasNextLine()) {
            try {
                String line = inFile.nextLine();
                lineNum++;
                avg = average(line);
                System.out.println("The average of the values on line " + lineNum + " is " + avg);
                validLines++;
            }
            catch(InputMismatchException i) {
                System.out.println("*** Error (line " + lineNum + "): Incorrect data type");
                corruptLines++;
            }
            catch (Exception e) {
                System.out.println("*** Error (line " + lineNum + "): " + e.getMessage());
                corruptLines++;
            }
        }
        System.out.println("\nThere were " + validLines + " valid lines of data\nThere were " + corruptLines +
                " corrupt lines of data");
    }
        public static double average (String line) throws Exception {
            Scanner sc = new Scanner(line);
            if(line.equals("")) throw new Exception("Line is empty - average can't be taken");
            int header = sc.nextInt();
            if(header == 0) throw new Exception("Header value of 0 - average can't be taken");
            if(header < 0) throw new Exception("Corrupt line - negative header value");
            int nums = 0, count = 0;
            while(sc.hasNextInt()) {
                nums += sc.nextInt();
                count++;
            }
            if(count < header) throw  new Exception("Corrupt line - fewer values than header");
            if(count > header) throw new Exception("Corrupt line - extra values on line");
            return (double) nums/header;
        }
}
