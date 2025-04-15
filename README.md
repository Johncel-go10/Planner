import java.awt.*;
import javax.swing.*;

public class planner {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Planner");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(500, 450);
        frame.setLayout(new BorderLayout());

        Color panelColor = new Color(138, 135, 255);

        JLabel title = new JLabel("To-do Planner", SwingConstants.CENTER);
        title.setFont(new Font("Arial", Font.BOLD, 24));
        title.setOpaque(true);
        title.setBackground(panelColor);
        frame.add(title, BorderLayout.NORTH);

        DefaultListModel<String> historyModel = new DefaultListModel<>();
        JList<String> historyList = new JList<>(historyModel);
        JScrollPane historyScroll = new JScrollPane(historyList);
        historyScroll.setBorder(BorderFactory.createTitledBorder("Task History"));
        historyScroll.setPreferredSize(new Dimension(240, 0));
        JPanel rightPanel = new JPanel(new BorderLayout());
        rightPanel.setBackground(panelColor);
        rightPanel.add(historyScroll, BorderLayout.CENTER);
        frame.add(rightPanel, BorderLayout.EAST);

        DefaultListModel<String> savedModel = new DefaultListModel<>();
        JList<String> savedList = new JList<>(savedModel);
        JScrollPane savedScroll = new JScrollPane(savedList);
        savedScroll.setBorder(BorderFactory.createTitledBorder("Saved Tasks"));
        savedScroll.setPreferredSize(new Dimension(250, 0));

        JButton editSaved = new JButton("Edit");
        JButton deleteSaved = new JButton("Delete");

        JPanel savedButtonsPanel = new JPanel(new GridLayout(2, 1));
        savedButtonsPanel.setBackground(panelColor);
        savedButtonsPanel.add(editSaved);
        savedButtonsPanel.add(deleteSaved);

        JPanel leftPanel = new JPanel(new BorderLayout());
        leftPanel.setBackground(panelColor);
        leftPanel.add(savedScroll, BorderLayout.CENTER);
        leftPanel.add(savedButtonsPanel, BorderLayout.SOUTH);
        frame.add(leftPanel, BorderLayout.WEST);

        JPanel bottomPanel = new JPanel();
        bottomPanel.setBackground(panelColor);
        JTextField input = new JTextField(15);
        JButton addSave = new JButton("Add & Save");
        bottomPanel.add(input);
        bottomPanel.add(addSave);
        frame.add(bottomPanel, BorderLayout.SOUTH);

        addSave.addActionListener(e -> {
            String text = input.getText().trim();
            if (!text.isEmpty()) {
                historyModel.addElement(text);
                savedModel.addElement(text);
                input.setText("");
            }
        });

        editSaved.addActionListener(e -> {
            int index = savedList.getSelectedIndex();
            if (index != -1) {
                String selected = savedModel.get(index);
                String edited = JOptionPane.showInputDialog(frame, "Edit saved task:", selected);
                if (edited != null && !edited.trim().isEmpty()) {
                    savedModel.set(index, edited.trim());
                }
            } else {
                JOptionPane.showMessageDialog(frame, "Select a saved task to edit.");
            }
        });

        deleteSaved.addActionListener(e -> {
            int index = savedList.getSelectedIndex();
            if (index != -1) {
                savedModel.remove(index);
            } else {
                JOptionPane.showMessageDialog(frame, "Select a saved task to delete.");
            }
        });

        frame.setVisible(true);
    }
}
