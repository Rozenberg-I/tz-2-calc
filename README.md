import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Введите выражение:");
        String input = scanner.nextLine();
        try {
            String result = calc(input);
            System.out.println("Результат: " + result);
        } catch(Exception e) {
            System.out.println(e.getMessage());
        }
    }
    public static String calc(String input) throws Exception {
        input = input.replaceAll("\\s+", "");
        String[] tokens = input.split("[+\\-*/]");
        if (tokens.length != 2) {
            throw new Exception("Некорректный ввод");
        }
        String operand1 = tokens[0];
        String operand2 = tokens[1];
        char operator = ' ';
        for (char c : input.toCharArray()) {
            if (c == '+' || c == '-' || c == '*' || c == '/') {
                operator = c;
                break;
            }
        }
        if (operator == ' ') {
            throw new Exception("Некорректный оператор");
        }
        boolean isRoman1 = isRomanNumeral(operand1);
        boolean isRoman2 = isRomanNumeral(operand2);
        boolean isArabic1 = isArabicNumeral(operand1);
        boolean isArabic2 = isArabicNumeral(operand2);
        if ((isRoman1 && isArabic2) || (isArabic1 && isRoman2)) {
            throw new Exception("Нельзя смешивать разные системы исчисления");
        }
        int num1;
        int num2;
        boolean isRoman = false;
        if (isRoman1 && isRoman2) {
            num1 = romanToArabic(operand1);
            num2 = romanToArabic(operand2);
            isRoman = true;
        } else if (isArabic1 && isArabic2) {
            num1 = Integer.parseInt(operand1);
            num2 = Integer.parseInt(operand2);
        } else {
            throw new Exception("Некорректные числа");
        }
        if (num1 < 1 || num1 > 10 || num2 < 1 || num2 > 10) {
            throw new Exception("Числа должны быть от 1 до 10");
        }
        int result = calculate(num1, num2, operator);
        if (isRoman && result < 1) {
            throw new Exception("В римской системе нет отрицательных чисел");
        }
        if (isRoman) {
            return arabicToRoman(result);
        } else {
            return Integer.toString(result);
        }
    }
    public static boolean isRomanNumeral(String s) {
        s = s.toUpperCase();
        return romanToArabic(s) != -1;
    }
    public static int romanToArabic(String s) {
        switch(s.toUpperCase()) {
            case "I": return 1;
            case "II": return 2;
            case "III": return 3;
            case "IV": return 4;
            case "V": return 5;
            case "VI": return 6;
            case "VII": return 7;
            case "VIII": return 8;
            case "IX": return 9;
            case "X": return 10;
            default: return -1;
        }
    }
    public static String arabicToRoman(int number) {
        if (number < 1 || number > 100) {
            throw new IllegalArgumentException("Результат вне диапазона (1-100)");
        }
        StringBuilder sb = new StringBuilder();
        while (number >= 100) {
            sb.append("C");
            number -= 100;
        }
        if (number >= 90) {
            sb.append("XC");
            number -= 90;
        }
        if (number >= 50) {
            sb.append("L");
            number -= 50;
        }
        if (number >= 40) {
            sb.append("XL");
            number -= 40;
        }
        while (number >= 10) {
            sb.append("X");
            number -= 10;
        }
        if (number == 9) {
            sb.append("IX");
            number -= 9;
        }
        if (number >= 5) {
            sb.append("V");
            number -= 5;
        }
        if (number == 4) {
            sb.append("IV");
            number -= 4;
        }
        while (number >= 1) {
            sb.append("I");
            number -= 1;
        }
        return sb.toString();
    }
    public static boolean isArabicNumeral(String s) {
        return s.matches("\\d+");
    }
    public static int calculate(int a, int b, char op) throws Exception {
        switch(op) {
            case '+': return a + b;
            case '-': return a - b;
            case '*': return a * b;
            case '/':
                if (b == 0) throw new Exception("Деление на ноль");
                return a / b;
            default:
                throw new Exception("Неизвестный оператор");
        }
    }
}
