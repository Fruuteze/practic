import java.math.BigDecimal;
import java.util.Scanner;

public class FinancialConsoleApp {
    static BigDecimal[] expenses = new BigDecimal[30];
    static final double EUR_RATE = 0.82;
    static final double USD_RATE = 1.0;
    static final double CNY_RATE = 6.45;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        while (true) {
            System.out.println("\nВыберите действие:");
            System.out.println("1 – Ввести расходы за определенный день");
            System.out.println("2 – Траты за месяц");
            System.out.println("3 – Самая большая сумма расхода за месяц");
            System.out.println("4 - Конвертер валюты");
            System.out.println("0 – Выход");
            int choice = scanner.nextInt();
            
            switch (choice) {
                case 1:
                    enterExpenses(scanner);
                    break;
                case 2:
                    displayExpenses();
                    break;
                case 3:
                    maxExpense();
                    break;
                case 4:
                    currencyConverter();
                    break;
                case 0:
                    System.out.println("До свидания!");
                    System.exit(0);
                    break;
                default:
                    System.out.println("Неверный выбор, попробуйте снова.");
                    break;
            }
        }
    }

    public static void enterExpenses(Scanner scanner) {
        System.out.println("Введите день (от 1 до 30):");
        int day = scanner.nextInt();
        System.out.println("Введите сумму трат за текущий день:");
        BigDecimal amount = scanner.nextBigDecimal();
        
        if (expenses[day-1] != null) {
            System.out.println("На этот день уже указана сумма трат: " + expenses[day-1]);
            System.out.println("Хотите перезаписать? (да/нет)");
            String answer = scanner.next();
            if (answer.equalsIgnoreCase("нет")) {
                return;
            }
        }
        
        expenses[day-1] = amount;
        
        System.out.println("Желаете ввести траты за другой день? (да/нет)");
        String anotherDay = scanner.next();
        if (anotherDay.equalsIgnoreCase("да")) {
            enterExpenses(scanner);
        }
    }

    public static void displayExpenses() {
        System.out.println("Траты за месяц:");
        for (int i = 0; i < expenses.length; i++) {
            if (expenses[i] != null) {
                System.out.println((i + 1) + " день – " + expenses[i] + " руб");
            }
        }
    }

    public static void maxExpense() {
        BigDecimal maxAmount = BigDecimal.ZERO;
        int maxDay = 0;

        for (int i = 0; i < expenses.length; i++) {
            if (expenses[i] != null && expenses[i].compareTo(maxAmount) > 0) {
                maxAmount = expenses[i];
                maxDay = i + 1;
            }
        }
        
        System.out.println("Самая большая сумма расхода за месяц: " + maxDay + " день – " + maxAmount + " руб");
    }

    public static void currencyConverter() {
        BigDecimal sum = BigDecimal.ZERO;
        for (int i = 0; i < expenses.length; i++) {
            if (expenses[i] != null) {
                sum = sum.add(expenses[i]);
            }
        }

        BigDecimal eur = sum.multiply(BigDecimal.valueOf(EUR_RATE));
        BigDecimal usd = sum.multiply(BigDecimal.valueOf(USD_RATE));
        BigDecimal cny = sum.multiply(BigDecimal.valueOf(CNY_RATE));

        System.out.println("Сумма трат за месяц:");
        System.out.println("В евро: " + eur);
        System.out.println("В долларах: " + usd);
        System.out.println("В юанях: " + cny);
    }
}
