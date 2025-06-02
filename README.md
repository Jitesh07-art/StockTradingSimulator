import java.awt.*;
import java.util.HashMap;
import javax.swing.*;

public class StockTradingSimulator {
    private static final HashMap<String, Double> stockPrices = new HashMap<>();
    private static final HashMap<String, HashMap<String, Integer>> userHoldings = new HashMap<>();

    public static void main(String[] args) {
        JFrame frame = new JFrame("Stock Trading Simulator");
        frame.setSize(900, 600);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.getContentPane().setBackground(new Color(230, 230, 250));
        frame.setLayout(null);

        JLabel titleLabel = new JLabel("Stock Trading Simulator");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 24));
        titleLabel.setBounds(300, 10, 400, 30);
        frame.add(titleLabel);

        JLabel userIdLabel = new JLabel("User ID:");
        userIdLabel.setBounds(50, 60, 100, 30);
        userIdLabel.setFont(new Font("Arial", Font.BOLD, 14));
        frame.add(userIdLabel);

        JTextField userIdField = new JTextField();
        userIdField.setBounds(130, 60, 160, 30);
        frame.add(userIdField);

        JButton loginButton = new JButton("Login");
        loginButton.setBounds(310, 60, 160, 30);
        loginButton.setBackground(Color.BLUE);
        loginButton.setForeground(Color.WHITE);
        frame.add(loginButton);

        JLabel stockLabel = new JLabel("Stock Symbol:");
        stockLabel.setBounds(50, 110, 120, 30);
        stockLabel.setFont(new Font("Arial", Font.BOLD, 14));
        frame.add(stockLabel);

        JTextField stockField = new JTextField();
        stockField.setBounds(170, 110, 100, 30);
        frame.add(stockField);

        JLabel quantityLabel = new JLabel("Quantity:");
        quantityLabel.setBounds(300, 110, 80, 30);
        quantityLabel.setFont(new Font("Arial", Font.BOLD, 14));
        frame.add(quantityLabel);

        JTextField quantityField = new JTextField();
        quantityField.setBounds(380, 110, 100, 30);
        frame.add(quantityField);

        JButton buyButton = new JButton("Buy Stock");
        buyButton.setBounds(130, 160, 160, 40);
        buyButton.setBackground(Color.GREEN);
        buyButton.setForeground(Color.WHITE);
        frame.add(buyButton);

        JButton sellButton = new JButton("Sell Stock");
        sellButton.setBounds(310, 160, 160, 40);
        sellButton.setBackground(Color.RED);
        sellButton.setForeground(Color.WHITE);
        frame.add(sellButton);

        JTextArea portfolioArea = new JTextArea();
        JScrollPane scrollPane1 = new JScrollPane(portfolioArea);
        scrollPane1.setBounds(550, 60, 300, 180);
        frame.add(scrollPane1);

        JLabel portfolioLabel = new JLabel("Portfolio");
        portfolioLabel.setBounds(550, 35, 150, 20);
        portfolioLabel.setFont(new Font("Arial", Font.BOLD, 14));
        frame.add(portfolioLabel);

        JTextArea marketArea = new JTextArea();
        JScrollPane scrollPane2 = new JScrollPane(marketArea);
        scrollPane2.setBounds(30, 250, 820, 270);
        frame.add(scrollPane2);

        JLabel marketLabel = new JLabel("Stock Market Prices");
        marketLabel.setBounds(30, 220, 250, 20);
        marketLabel.setFont(new Font("Arial", Font.BOLD, 16));
        frame.add(marketLabel);

        // Stock prices
        stockPrices.put("MSFT", 320.4);
        stockPrices.put("GOOGL", 2800.5);
        stockPrices.put("AAPL", 175.3);
        stockPrices.put("TSLA", 850.75);
        stockPrices.put("AMZN", 3342.88);
        stockPrices.put("NFLX", 498.66);
        stockPrices.put("META", 299.15);
        stockPrices.put("NVDA", 835.12);
        stockPrices.put("BABA", 92.34);
        stockPrices.put("INTC", 34.55);

        loginButton.addActionListener(e -> {
            String userId = userIdField.getText();
            if (!userHoldings.containsKey(userId)) {
                userHoldings.put(userId, new HashMap<>());
            }
            updatePortfolio(userId, portfolioArea);
            updateMarketTable(userId, marketArea);
        });

        buyButton.addActionListener(e -> {
            String userId = userIdField.getText();
            String symbol = stockField.getText();
            int qty = Integer.parseInt(quantityField.getText());

            userHoldings.get(userId).put(symbol,
                userHoldings.get(userId).getOrDefault(symbol, 0) + qty);

            updatePortfolio(userId, portfolioArea);
            updateMarketTable(userId, marketArea);
        });

        sellButton.addActionListener(e -> {
            String userId = userIdField.getText();
            String symbol = stockField.getText();
            int qty = Integer.parseInt(quantityField.getText());
            int currentQty = userHoldings.get(userId).getOrDefault(symbol, 0);

            if (currentQty >= qty) {
                userHoldings.get(userId).put(symbol, currentQty - qty);
            }

            updatePortfolio(userId, portfolioArea);
            updateMarketTable(userId, marketArea);
        });

        frame.setVisible(true);
    }

    private static void updatePortfolio(String userId, JTextArea area) {
        area.setText("User ID: " + userId + "\nStock\t\tQuantity\n----------------------\n");
        HashMap<String, Integer> holdings = userHoldings.get(userId);
        for (String stock : holdings.keySet()) {
            int qty = holdings.get(stock);
            if (qty > 0)
                area.append(stock + "\t\t" + qty + "\n");
        }
    }

    private static void updateMarketTable(String userId, JTextArea area) {
        area.setText("Stock\t\tPrice\t\tYour Holdings\n-----------------------------------------\n");
        for (String stock : stockPrices.keySet()) {
            double price = stockPrices.get(stock);
            int qty = userHoldings.get(userId).getOrDefault(stock, 0);
            area.append(stock + "\t\t$" + price + "\t\t" + qty + "\n");
        }
    }
}
