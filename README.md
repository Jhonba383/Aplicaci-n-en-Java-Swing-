import javax.swing.*;
import java.awt.*;
import java.util.ArrayList;

public class TaskManagerApp {
    private static final DefaultListModel<String> taskListModel = new DefaultListModel<>();
    private static final JList<String> taskList = new JList<>(taskListModel);
    private static final ArrayList<String> taskDescriptions = new ArrayList<>();
    private static DefaultListModel<String> trashListModel = new DefaultListModel<>();
    private static final ArrayList<String> trashDescriptions = new ArrayList<>();

    public static void main(String[] args) {
        SwingUtilities.invokeLater(TaskManagerApp::createMainWindow);
    }

    private static void createMainWindow() {
        JFrame frame = new JFrame("Gestor de Tareas");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(500, 400);
        
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());
        
        // Agregar imagen en la parte superior
        ImageIcon icon = new ImageIcon("logo.png"); // Asegúrate de que la imagen esté en la misma carpeta del proyecto
        JLabel logoLabel = new JLabel(icon, SwingConstants.CENTER);
        panel.add(logoLabel, BorderLayout.NORTH);
        
        taskList.setSelectionMode(ListSelectionModel.SINGLE_SELECTION);
        panel.add(new JScrollPane(taskList), BorderLayout.CENTER);
        
        JPanel buttonPanel = new JPanel();
        JButton addButton = new JButton("Agregar Tarea");
        JButton viewButton = new JButton("Ver Detalles");
        JButton deleteButton = new JButton("Eliminar Tarea");
        JButton trashButton = new JButton("Papelera");
        JButton allNotesButton = new JButton("Todas las Notas");
        JButton manageTasksButton = new JButton("Administrar Tareas");
        
        buttonPanel.add(addButton);
        buttonPanel.add(viewButton);
        buttonPanel.add(deleteButton);
        buttonPanel.add(trashButton);
        buttonPanel.add(allNotesButton);
        buttonPanel.add(manageTasksButton);
        
        panel.add(buttonPanel, BorderLayout.SOUTH);
        
        addButton.addActionListener(e -> openAddTaskWindow());
        viewButton.addActionListener(e -> openTaskDetailWindow());
        deleteButton.addActionListener(e -> moveToTrash());
        trashButton.addActionListener(e -> openTrashWindow());
        allNotesButton.addActionListener(e -> showAllNotes());
        manageTasksButton.addActionListener(e -> openManageTasksWindow());
        
        frame.add(panel);
        frame.setVisible(true);
    }

    private static void openAddTaskWindow() {
        JFrame addFrame = new JFrame("Agregar Tarea");
        addFrame.setSize(350, 250);
        
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(4, 1));
        
        JTextField taskField = new JTextField();
        JTextArea descriptionField = new JTextArea(3, 20);
        JButton addButton = new JButton("Agregar");
        
        panel.add(new JLabel("Nombre de la Tarea:"));
        panel.add(taskField);
        panel.add(new JLabel("Descripción de la Tarea:"));
        panel.add(new JScrollPane(descriptionField));
        panel.add(addButton);
        
        addButton.addActionListener(e -> {
            String taskName = taskField.getText().trim();
            String description = descriptionField.getText().trim();
            if (!taskName.isEmpty()) {
                taskListModel.addElement(taskName);
                taskDescriptions.add(description);
                addFrame.dispose();
            }
        });
        
        addFrame.add(panel);
        addFrame.setVisible(true);
    }

    private static void openTaskDetailWindow() {
        int selectedIndex = taskList.getSelectedIndex();
        if (selectedIndex == -1) {
            JOptionPane.showMessageDialog(null, "Seleccione una tarea");
            return;
        }
        
        JFrame detailFrame = new JFrame("Detalles de la Tarea");
        detailFrame.setSize(350, 200);
        
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());
        
        JLabel taskLabel = new JLabel("Tarea: " + taskListModel.getElementAt(selectedIndex));
        JTextArea descriptionArea = new JTextArea(taskDescriptions.get(selectedIndex));
        descriptionArea.setEditable(false);
        
        panel.add(taskLabel, BorderLayout.NORTH);
        panel.add(new JScrollPane(descriptionArea), BorderLayout.CENTER);
        
        detailFrame.add(panel);
        detailFrame.setVisible(true);
    }

    private static void moveToTrash() {
        int selectedIndex = taskList.getSelectedIndex();
        if (selectedIndex != -1) {
            trashListModel.addElement(taskListModel.get(selectedIndex));
            trashDescriptions.add(taskDescriptions.get(selectedIndex));
            taskListModel.remove(selectedIndex);
            taskDescriptions.remove(selectedIndex);
        } else {
            JOptionPane.showMessageDialog(null, "Seleccione una tarea para eliminar");
        }
    }

    private static void openTrashWindow() {
        JFrame trashFrame = new JFrame("Papelera");
        trashFrame.setSize(300, 200);
        JList<String> trashList = new JList<>(trashListModel);
        trashFrame.add(new JScrollPane(trashList));
        trashFrame.setVisible(true);
    }

    private static void showAllNotes() {
        JOptionPane.showMessageDialog(null, "Total de tareas: " + taskListModel.size());
    }

    private static void openManageTasksWindow() {
        JFrame manageFrame = new JFrame("Administrar Tareas");
        manageFrame.setSize(400, 300);
        
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(2, 1));
        
        JLabel label = new JLabel("Aquí puedes administrar tus tareas", SwingConstants.CENTER);
        JButton closeButton = new JButton("Cerrar");
        
        panel.add(label);
        panel.add(closeButton);
        
        closeButton.addActionListener(e -> manageFrame.dispose());
        
        manageFrame.add(panel);
        manageFrame.setVisible(true);
    }
}