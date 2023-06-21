 trabalhopilha
 package calculpilha;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Stack;

public class Calculpilha extends JFrame implements ActionListener {
  // texto para mostrar resultados
  private JTextField display;
  // Pilha para armazenar os numeros
  private Stack<Integer> pilha;

  public Calculpilha() {
    setTitle("Calculadora");
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    setResizable(false);

    pilha = new Stack<>();

    display = new JTextField(15);
    display.setEditable(false);
    display.setHorizontalAlignment(JTextField.RIGHT);
    display.setFont(new Font("Arial", Font.PLAIN, 20));

    JPanel panel = new JPanel(new BorderLayout());
    panel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
    panel.add(display, BorderLayout.NORTH);
    panel.add(createButtonPanel(), BorderLayout.CENTER);

    getContentPane().add(panel);
    pack();
  }

  private JPanel createButtonPanel() {
    JPanel panel = new JPanel(new GridLayout(4, 4, 5, 5));

    String[] buttons = {
        "7", "8", "9", "÷",
        "4", "5", "6", "×",
        "1", "2", "3", "-",
        "0", "C", "=", "+"
    };

    for (String button : buttons) {
      JButton btn = new JButton(button);
      btn.setFont(new Font("Arial", Font.PLAIN, 20));
      btn.addActionListener(this);
      panel.add(btn);
    }

    return panel;
  }

  public void actionPerformed(ActionEvent e) {
    String command = e.getActionCommand();

    switch (command) {
      case "C":
        display.setText("");
        pilha.clear();
        break;
      case "=":
        String expressao = display.getText();
        int resultado = calcularExpressao(expressao);
        display.setText(Integer.toString(resultado));
        pilha.clear();
        pilha.push(resultado);
        break;
      default:
        display.setText(display.getText() + command);
        break;
    }
  }

  public int calcularExpressao(String expressao) {
    String[] tokens = expressao.split(" ");

    for (String token : tokens) {
      if (isNumber(token)) {
        int numero = Integer.parseInt(token);
        pilha.push(numero);
      } else if (isOperator(token)) {
        // retirar num2 da pilha
        int num2 = pilha.pop();
        // retirar num1 da pilha
        int num1 = pilha.pop();
        int resultado = 0;

        switch (token) {
          // operação de adição e empilha o resultado
          case "+":
            resultado = num1 + num2;
            break;
          // operação de subtracao e empilha o resultado
          case "-":
            resultado = num1 - num2;
            break;
          // operação de multiplicacao e empilha o resultado
          case "*":
            resultado = num1 * num2;
            break;
          // operação de dividir e empilha o resultado
          case "/":
            resultado = num1 / num2;
            break;
        }

        pilha.push(resultado);
      }
    }

    return pilha.pop();
  }

  private boolean isNumber(String token) {
    try {
      Integer.parseInt(token);
      return true;
    } catch (NumberFormatException e) {
      return false;
    }
  }

  private boolean isOperator(String token) {
    return token.equals("+") || token.equals("-") || token.equals("*") || token.equals("/");
  }

  public static void main(String[] args) {
    SwingUtilities.invokeLater(new Runnable() {
      public void run() {
        new Calculpilha().setVisible(true);
      }
    });
  }
}
