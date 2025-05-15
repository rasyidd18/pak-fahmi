import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.HashSet;
import java.util.Set;

public class MovieBookingUI {
    static JFrame frame;
    static JPanel seatPanel;
    static JLabel selectedMovieLabel;
    static String selectedMovie = null;
    static String selectedTime = null;
    static Set<JToggleButton> selectedSeats = new HashSet<>();

    public static void main(String[] args) {
        frame = new JFrame("Movie Booking UI");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(360, 750);
        frame.getContentPane().setBackground(Color.BLACK);
        frame.setLayout(null);

        // Header lokasi
        JLabel locationLabel = new JLabel("Jakarta");
        locationLabel.setBounds(20, 20, 200, 25);
        locationLabel.setForeground(Color.CYAN);
        locationLabel.setFont(new Font("Arial", Font.BOLD, 16));
        frame.add(locationLabel);

        JButton dropdownButton = new JButton("˅");
        dropdownButton.setBounds(220, 20, 50, 25);
        dropdownButton.setForeground(Color.CYAN);
        dropdownButton.setBackground(Color.BLACK);
        dropdownButton.setBorder(null);
        frame.add(dropdownButton);

        dropdownButton.addActionListener(e -> {
            String[] cities = {"Jakarta", "Bandung", "Surabaya", "Yogyakarta"};
            String selectedCity = (String) JOptionPane.showInputDialog(frame, "Pilih Kota:", "Lokasi",
                    JOptionPane.PLAIN_MESSAGE, null, cities, locationLabel.getText());
            if (selectedCity != null) {
                locationLabel.setText(selectedCity);
            }
        });

        // Search bar
        JTextField searchBar = new JTextField("Search movies...");
        searchBar.setBounds(20, 60, 300, 30);
        searchBar.setBackground(new Color(30, 30, 30));
        searchBar.setForeground(Color.LIGHT_GRAY);
        frame.add(searchBar);

        // Now Playing section
        JLabel nowPlaying = new JLabel("Now Playing:");
        nowPlaying.setBounds(20, 100, 200, 20);
        nowPlaying.setForeground(Color.CYAN);
        nowPlaying.setFont(new Font("Arial", Font.BOLD, 14));
        frame.add(nowPlaying);

        // Panel tombol film
        String[] movies = {"Avatar 2", "John Wick 4", "Oppenheimer"};
        int y = 130;
        for (String movie : movies) {
            JButton movieButton = new JButton(movie);
            movieButton.setBounds(20, y, 300, 30);
            movieButton.setBackground(Color.DARK_GRAY);
            movieButton.setForeground(Color.CYAN);
            frame.add(movieButton);

            movieButton.addActionListener(e -> {
                selectedMovie = movie;
                selectedMovieLabel.setText("Film dipilih: " + movie);
                selectedTime = null;
                JOptionPane.showMessageDialog(frame, "Film dipilih: " + movie);
                generateSeats();
            });

            y += 40;
        }

        // Label film dipilih
        selectedMovieLabel = new JLabel("Film dipilih: -");
        selectedMovieLabel.setBounds(20, y, 300, 20);
        selectedMovieLabel.setForeground(Color.WHITE);
        frame.add(selectedMovieLabel);

        // Waktu tayang
        JLabel timeLabel = new JLabel("Pilih Waktu Tayang:");
        timeLabel.setBounds(20, y + 30, 200, 20);
        timeLabel.setForeground(Color.CYAN);
        frame.add(timeLabel);

        String[] times = {"17:00", "19:00", "19:30"};
        JComboBox<String> timeCombo = new JComboBox<>(times);
        timeCombo.setBounds(170, y + 30, 150, 25);
        frame.add(timeCombo);

        timeCombo.addActionListener(e -> {
            selectedTime = (String) timeCombo.getSelectedItem();
        });

        // Panel tempat duduk
        seatPanel = new JPanel();
        seatPanel.setLayout(new GridLayout(5, 5, 5, 5));
        seatPanel.setBounds(20, y + 70, 300, 200);
        seatPanel.setBackground(Color.BLACK);
        frame.add(seatPanel);

        // Tombol pesan
        JButton bookButton = new JButton("Pesan Tiket");
        bookButton.setBounds(100, y + 280, 150, 30);
        bookButton.setBackground(new Color(0, 120, 215));
        bookButton.setForeground(Color.WHITE);
        frame.add(bookButton);

        bookButton.addActionListener(e -> {
            if (selectedMovie == null) {
                JOptionPane.showMessageDialog(frame, "Silakan pilih film terlebih dahulu.");
                return;
            }
            if (selectedTime == null) {
                JOptionPane.showMessageDialog(frame, "Silakan pilih waktu tayang terlebih dahulu.");
                return;
            }
            if (selectedSeats.isEmpty()) {
                JOptionPane.showMessageDialog(frame, "Silakan pilih kursi terlebih dahulu.");
                return;
            }

            // Ambil kursi yang dipilih
            StringBuilder seatNames = new StringBuilder();
            for (JToggleButton seat : selectedSeats) {
                seatNames.append(seat.getText()).append(" ");
            }

            int jumlahKursi = selectedSeats.size();
            int hargaTiket = jumlahKursi * 40000;
            String kursiText = jumlahKursi + " tempat duduk (" + seatNames.toString().trim() + ")";

            // Panel custom tiket
            JPanel ticketPanel = new JPanel();
            ticketPanel.setLayout(new BoxLayout(ticketPanel, BoxLayout.Y_AXIS));
            ticketPanel.setBackground(Color.BLACK);
            ticketPanel.setBorder(BorderFactory.createLineBorder(Color.CYAN, 2));
            ticketPanel.setPreferredSize(new Dimension(300, 180));

            JLabel iconLabel = new JLabel("🎟", SwingConstants.CENTER);
            iconLabel.setFont(new Font("Arial", Font.PLAIN, 40));
            iconLabel.setAlignmentX(Component.CENTER_ALIGNMENT);

            JLabel titleLabel = new JLabel("Tiket berhasil dipesan!");
            titleLabel.setFont(new Font("Arial", Font.BOLD, 16));
            titleLabel.setForeground(Color.CYAN);
            titleLabel.setAlignmentX(Component.CENTER_ALIGNMENT);

            JLabel movieLabel = new JLabel("Film: " + selectedMovie);
            movieLabel.setFont(new Font("Arial", Font.PLAIN, 14));
            movieLabel.setForeground(Color.WHITE);
            movieLabel.setAlignmentX(Component.CENTER_ALIGNMENT);

            JLabel timeLabelInfo = new JLabel("Jam: " + selectedTime);
            timeLabelInfo.setFont(new Font("Arial", Font.PLAIN, 14));
            timeLabelInfo.setForeground(Color.WHITE);
            timeLabelInfo.setAlignmentX(Component.CENTER_ALIGNMENT);

            JLabel seatLabel = new JLabel("Kursi: " + kursiText);
            seatLabel.setFont(new Font("Arial", Font.PLAIN, 14));
            seatLabel.setForeground(Color.WHITE);
            seatLabel.setAlignmentX(Component.CENTER_ALIGNMENT);

            JLabel priceLabel = new JLabel("Total: Rp" + hargaTiket);
            priceLabel.setFont(new Font("Arial", Font.BOLD, 14));
            priceLabel.setForeground(Color.GREEN);
            priceLabel.setAlignmentX(Component.CENTER_ALIGNMENT);

            ticketPanel.add(Box.createVerticalStrut(10));
            ticketPanel.add(iconLabel);
            ticketPanel.add(titleLabel);
            ticketPanel.add(movieLabel);
            ticketPanel.add(timeLabelInfo);
            ticketPanel.add(seatLabel);
            ticketPanel.add(priceLabel);

            JOptionPane.showMessageDialog(frame, ticketPanel, "Tiket Bioskop", JOptionPane.PLAIN_MESSAGE);

            // Reset
            selectedSeats.clear();
            generateSeats();
        });

        frame.setVisible(true);
    }

    // Membuat ulang kursi saat film dipilih
    static void generateSeats() {
        seatPanel.removeAll();
        selectedSeats.clear();
        char[] rows = {'A', 'B', 'C', 'D', 'E'};
        for (int i = 0; i < 5; i++) {
            for (int j = 1; j <= 5; j++) {
                String seatCode = rows[i] + String.valueOf(j);
                JToggleButton seat = new JToggleButton(seatCode);
                seat.setBackground(Color.DARK_GRAY);
                seat.setForeground(Color.WHITE);
                seat.addActionListener(e -> {
                    if (seat.isSelected()) {
                        seat.setBackground(Color.BLUE);
                        selectedSeats.add(seat);
                    } else {
                        seat.setBackground(Color.DARK_GRAY);
                        selectedSeats.remove(seat);
                    }
                });
                seatPanel.add(seat);
            }
        }
        seatPanel.revalidate();
        seatPanel.repaint();
    }
}
