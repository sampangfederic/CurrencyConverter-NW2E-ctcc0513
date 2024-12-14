import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.HashMap;
import java.util.Map;

// Abstract Data Type for Currency Conversion
class CurrencyConverterADT {
    private Map<String, Double> exchangeRates;

    public CurrencyConverterADT() {
        exchangeRates = new HashMap<>();
    }

    public void setExchangeRate(String key, double rate) {
        exchangeRates.put(key, rate);
    }

    public double convert(String fromToKey, double amount) {
        return exchangeRates.getOrDefault(fromToKey, 0.0) * amount;
    }
}

public class CurrencyConverterGUI {

    private static CurrencyConverterADT converter;

    public static void main(String[] args) {
        // Initialize the converter and exchange rates
        converter = new CurrencyConverterADT();
        initializeExchangeRates();

        // Create GUI components
        JFrame frame = new JFrame("Currency Converter");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(800, 600);
        frame.setLayout(new BorderLayout());

        JLabel title = new JLabel("Currency Converter", SwingConstants.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 20));
        frame.add(title, BorderLayout.NORTH);

        JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new GridLayout(3, 2, 10, 10));

        JLabel amountLabel = new JLabel("Enter Amount:");
        JTextField amountField = new JTextField();
        JLabel fromCurrencyLabel = new JLabel("From Currency:");
        JComboBox<String> fromCurrency = new JComboBox<>(new String[]{"PHP", "USD", "EUR", "SAR", "BHD"});
        JLabel toCurrencyLabel = new JLabel("To Currency:");
        JComboBox<String> toCurrency = new JComboBox<>(new String[]{"PHP", "USD", "EUR", "SAR", "BHD"});

        inputPanel.add(amountLabel);
        inputPanel.add(amountField);
        inputPanel.add(fromCurrencyLabel);
        inputPanel.add(fromCurrency);
        inputPanel.add(toCurrencyLabel);
        inputPanel.add(toCurrency);

        frame.add(inputPanel, BorderLayout.CENTER);

        JPanel buttonPanel = new JPanel();
        JButton convertButton = new JButton("Convert");
        JLabel resultLabel = new JLabel("Result: ", SwingConstants.CENTER);

        convertButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                try {
                    double amount = Double.parseDouble(amountField.getText());
                    String from = fromCurrency.getSelectedItem().toString();
                    String to = toCurrency.getSelectedItem().toString();

                    if (from.equals(to)) {
                        resultLabel.setText("Result: " + amount + " " + to);
                    } else {
                        String key = from + ":" + to;
                        double convertedAmount = converter.convert(key, amount);

                        if (convertedAmount == 0.0) {
                            resultLabel.setText("Conversion rate not available.");
                        } else {
                            resultLabel.setText("Result: " + String.format("%.3f", convertedAmount) + " " + to);
                        }
                    }
                } catch (NumberFormatException ex) {
                    resultLabel.setText("Invalid input. Please enter a valid number.");
                }
            }
        });

        buttonPanel.setLayout(new BorderLayout());
        buttonPanel.add(convertButton, BorderLayout.NORTH);
        buttonPanel.add(resultLabel, BorderLayout.CENTER);

        frame.add(buttonPanel, BorderLayout.SOUTH);

        frame.setVisible(true);
    }

    private static void initializeExchangeRates() {
        // Exchange rates (as of some fixed date, modify as needed)
        converter.setExchangeRate("PHP:USD", 0.020);
        converter.setExchangeRate("PHP:EUR", 0.017);
        converter.setExchangeRate("PHP:SAR", 0.073);
        converter.setExchangeRate("PHP:BHD", 0.0074);

        converter.setExchangeRate("USD:PHP", 51.23);
        converter.setExchangeRate("USD:EUR", 0.87);
        converter.setExchangeRate("USD:SAR", 3.75);
        converter.setExchangeRate("USD:BHD", 0.38);

        converter.setExchangeRate("EUR:PHP", 58.75);
        converter.setExchangeRate("EUR:USD", 1.15);
        converter.setExchangeRate("EUR:SAR", 4.30);
        converter.setExchangeRate("EUR:BHD", 0.43);

        converter.setExchangeRate("SAR:PHP", 13.66);
        converter.setExchangeRate("SAR:USD", 0.27);
        converter.setExchangeRate("SAR:EUR", 0.23);
        converter.setExchangeRate("SAR:BHD", 0.10);

        converter.setExchangeRate("BHD:PHP", 135.84);
        converter.setExchangeRate("BHD:USD", 2.65);
        converter.setExchangeRate("BHD:EUR", 2.31);
        converter.setExchangeRate("BHD:SAR", 9.95);
    }
}
