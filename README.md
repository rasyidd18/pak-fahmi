# pak-fahmi
Pengumpulan Tugas Apps 
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
    static Set<JToggleButton> selectedSeats = new HashSet<>();

    public static void main(String[] args) {
        frame = new JFrame("Movie Booking UI");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(360, 700);
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

        // Panel tempat duduk
        seatPanel = new JPanel();
        seatPanel.setLayout(new GridLayout(5, 5, 5, 5));
        seatPanel.setBounds(20, y + 30, 300, 200);
        seatPanel.setBackground(Color.BLACK);
        frame.add(seatPanel);

        // Tombol pesan
        JButton bookButton = new JButton("Pesan Tiket");
        bookButton.setBounds(100, y + 250, 150, 30);
        bookButton.setBackground(new Color(0, 120, 215));
        bookButton.setForeground(Color.WHITE);
        frame.add(bookButton);

        bookButton.addActionListener(e -> {
            if (selectedMovie == null) {
                JOptionPane.showMessageDialog(frame, "Silakan pilih film terlebih dahulu.");
                return;
            }
            if (selectedSeats.isEmpty()) {
                JOptionPane.showMessageDialog(frame, "Silakan pilih kursi terlebih dahulu.");
                return;
            }
            JOptionPane.showMessageDialog(frame, "Tiket berhasil dipesan!\nFilm: " + selectedMovie +
                    "\nKursi: " + selectedSeats.size() + " tempat duduk.");
            selectedSeats.clear();
            generateSeats(); // Reset pilihan
        });

        frame.setVisible(true);
    }

    // Membuat ulang kursi saat film dipilih
    static void generateSeats() {
        seatPanel.removeAll();
        selectedSeats.clear();
        for (int i = 1; i <= 25; i++) {
            JToggleButton seat = new JToggleButton("K" + i);
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
        seatPanel.revalidate();
        seatPanel.repaint();
    }
}
