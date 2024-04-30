import org.w3c.dom.ls.LSOutput;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import javax.swing.border.*;
import java.sql.*;



public class CronometroComLapPararVoltas extends JFrame {
    private JLabel label;
    private Timer timer;
    private int segundos;
    private int volta;
    private boolean pararDepoisDaVolta;
    private JPanel painelVoltas;
    private DefaultListModel<String> modeloLista;
    private  String tempoVolta1;
    private  String tempoVolta2;
    private String tempoTotal;
    
  


    public CronometroComLapPararVoltas() {
        //Configuração da interface e botão iniciar
        super("Cronômetro com Lap, Parar e Voltas");
        label = new JLabel("00:00:00");
        label.setFont(new Font("Impact", Font.PLAIN, 40));
        label.setHorizontalAlignment(JLabel.CENTER);
        add(label, BorderLayout.CENTER);
        JButton botaoIniciar = new JButton("Iniciar");
        botaoIniciar.setBackground(new Color(165,250,182));
        botaoIniciar.setFont(new Font("Impact", Font.PLAIN, 30));
        
        //funcionalidade após ativação do botão iniciar
        botaoIniciar.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (timer == null) {
                    segundos = 0;
                    volta = 1;
                    pararDepoisDaVolta = false;
                    timer = new Timer(1000, new ActionListener() {
                        public void actionPerformed(ActionEvent e) {
                            segundos++;
                            int horas = segundos / 3600;
                            int minutos = (segundos % 3600) / 60;
                            int segs = segundos % 60;
                            label.setText(String.format("%02d:%02d:%02d", horas, minutos, segs));
                            if (pararDepoisDaVolta && volta == 3) {
                                timer.stop();
                                timer = null;
                                JOptionPane.showMessageDialog(CronometroComLapPararVoltas.this, "Fim do cronômetro");
                                JOptionPane.showMessageDialog(CronometroComLapPararVoltas.this, "O Tempo Total da Corrida é: "+tempoTotal);
                                
                                //Conexão do banco banco de dados e envio dos dados
                                try {
                                    Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/cronometro_teste", "root", "senha");

                                    // Insere as variáveis tempoVolta1 e tempoVolta2 na tabela 
                                    String query = "INSERT INTO voltas (volta1, volta2) VALUES (?, ?)";
                                    PreparedStatement statement = conn.prepareStatement(query);
                                    statement.setString(1, tempoVolta1);
                                    statement.setString(2, tempoVolta2);
                                    statement.executeUpdate();

                                    // Feche a conexão com o banco de dados
                                    conn.close();
                                } catch (SQLException ex) {
                                    ex.printStackTrace();
                                }
                            }
                        }
                    });
                    timer.start();
                }
            }
        });
        add(botaoIniciar, BorderLayout.NORTH);
        
        //Logica por trás do botão Parar
        JButton botaoParar = new JButton("Parar");
        botaoParar.setBackground(new Color(239,149,142));
        botaoParar.setFont(new Font("Impact", Font.PLAIN, 20));
        botaoParar.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (timer != null) {
                    timer.stop();
                    timer = null;
                }
            }
        });
        add(botaoParar, BorderLayout.WEST);
        
        //Logica para marcar as voltas após acionar o botão
        JButton botaoLap = new JButton("Volta");
        botaoLap.setBackground(new Color(255,255,173));
        botaoLap.setFont(new Font("Impact", Font.PLAIN, 20));
        botaoLap.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (timer != null) {
                    String tempoVolta = label.getText();
                    modeloLista.addElement("Volta " + volta + ": " + tempoVolta);
                    if (volta == 1) {
                        tempoVolta1 = tempoVolta; // atualiza a variável tempoVolta1
                    } else if (volta == 2) {
                        tempoVolta2 = tempoVolta; // atualiza a variável tempoVolta2
                        tempoTotal =tempoVolta2;
                        pararDepoisDaVolta = true;
                    }
                    volta++;
                }
            }
        });
        add(botaoLap, BorderLayout.EAST);
        
        //Configuração do painel para mostrar a volta e seu Tempo
        modeloLista = new DefaultListModel<String>();
        JList<String> lista = new JList<String>(modeloLista);
        JScrollPane scrollPane = new JScrollPane(lista);
        scrollPane.setPreferredSize(new Dimension(50, 100));
        Border borda = BorderFactory.createTitledBorder("Voltas");
        scrollPane.setBorder(borda);        

        painelVoltas = new JPanel(new BorderLayout());
        
        painelVoltas.add(scrollPane, BorderLayout.CENTER);
        painelVoltas.setPreferredSize(new Dimension(50, 100));
        

        add(painelVoltas, BorderLayout.SOUTH);

        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 400);
        setLocationRelativeTo(null);
        setVisible(true);
    }
    
    


    public static void main(String[] args) {  
        new CronometroComLapPararVoltas();
 
    }
}
