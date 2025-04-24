import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.HashMap;
class Tarea {
String nombre, curso, fecha;
boolean completada;
public Tarea(String nombre, String curso, String fecha) {
this.nombre = nombre;
this.curso = curso;
this.fecha = fecha;
this.completada = false;
}
public String toString() {
return nombre + " - " + curso + " - " + fecha + (completada ? " " : " ");
}
}
class Materia {
String nombre;
int creditos;
public Materia(String nombre, int creditos) {
this.nombre = nombre;
this.creditos = creditos;
}
public String toString() {
return nombre + " - Créditos: " + creditos;
}
}
public class SistemaDeTareasGUI extends JFrame {
private DefaultListModel<Tarea> modeloTareas = new DefaultListModel<>();
private JList<Tarea> listaTareas = new JList<>(modeloTareas);
private ArrayList<Materia> listaMaterias = new ArrayList<>();

private HashMap<String, String[]> datosCarnet = new HashMap<>();
public SistemaDeTareasGUI() {
setTitle("Sistema Académico - Politécnico G.C.");
setSize(650, 500);
setDefaultCloseOperation(EXIT_ON_CLOSE);
setLayout(new BorderLayout());
// Botones principales
JButton btnAgregarTarea = new JButton("Agregar tarea");
JButton btnCompletar = new JButton("Marcar como completada");
JButton btnAnalisis = new JButton("Ver análisis");
JButton btnAgregarMateria = new JButton("Agregar materia");
JButton btnVerMaterias = new JButton("Ver materias");
JButton btnVerCarnet = new JButton("Ver carnet");
JPanel panelBotones = new JPanel(new GridLayout(2, 3, 5, 5));
panelBotones.add(btnAgregarTarea);
panelBotones.add(btnCompletar);
panelBotones.add(btnAnalisis);
panelBotones.add(btnAgregarMateria);
panelBotones.add(btnVerMaterias);
panelBotones.add(btnVerCarnet);
add(new JScrollPane(listaTareas), BorderLayout.CENTER);
add(panelBotones, BorderLayout.SOUTH);
// Eventos
btnAgregarTarea.addActionListener(e -> agregarTarea());
btnCompletar.addActionListener(e -> marcarComoCompletada());
btnAnalisis.addActionListener(e -> mostrarAnalisis());
btnAgregarMateria.addActionListener(e -> agregarMateria());
btnVerMaterias.addActionListener(e -> mostrarMaterias());
btnVerCarnet.addActionListener(e -> mostrarCarnet());
// Datos simulados de carnet
datosCarnet.put("12345", new String[]{"Laura Pérez", "Ingeniería de Sistemas"});
datosCarnet.put("67890", new String[]{"Carlos Gómez", "Administración"});

}
private void agregarTarea() {
String nombre = JOptionPane.showInputDialog(this, "Nombre de la tarea:");
String curso = JOptionPane.showInputDialog(this, "Curso:");
String fecha = JOptionPane.showInputDialog(this, "Fecha límite (ej: 2025-04-10):");
if (nombre != null && curso != null && fecha != null) {
modeloTareas.addElement(new Tarea(nombre, curso, fecha));
}
}
private void marcarComoCompletada() {
int index = listaTareas.getSelectedIndex();
if (index != -1) {
Tarea t = modeloTareas.getElementAt(index);
t.completada = true;
listaTareas.repaint();
}
}
private void mostrarAnalisis() {
int total = modeloTareas.size();
int completadas = 0;
for (int i = 0; i < total; i++) {
if (modeloTareas.get(i).completada) completadas++;
}
JOptionPane.showMessageDialog(this,
"Tareas totales: " + total +
"\nTareas completadas: " + completadas +
"\nProgreso: " + (total > 0 ? (completadas * 100 / total) + "%" : "0%"),
"Análisis de Productividad", JOptionPane.INFORMATION_MESSAGE);
}
private void agregarMateria() {
String nombre = JOptionPane.showInputDialog(this, "Nombre de la materia:");
String credStr = JOptionPane.showInputDialog(this, "Número de créditos:");
try {
int creditos = Integer.parseInt(credStr);

if (nombre != null && !nombre.isEmpty()) {
listaMaterias.add(new Materia(nombre, creditos));
}
} catch (NumberFormatException e) {
JOptionPane.showMessageDialog(this, "Ingresa un número válido de créditos.");
}
}
private void mostrarMaterias() {
StringBuilder sb = new StringBuilder();
for (Materia m : listaMaterias) {
sb.append(m.toString()).append("\n");
}
JOptionPane.showMessageDialog(this, sb.toString(), "Materias inscritas",
JOptionPane.INFORMATION_MESSAGE);
}
private void mostrarCarnet() {
String numero = JOptionPane.showInputDialog(this, "Ingresa tu número de carnet:");
if (datosCarnet.containsKey(numero)) {
String[] datos = datosCarnet.get(numero);
JOptionPane.showMessageDialog(this,
"Nombre: " + datos[0] + "\nPrograma: " + datos[1] + "\nCarnet: " + numero,
"Carnet Estudiantil", JOptionPane.INFORMATION_MESSAGE);
} else {
JOptionPane.showMessageDialog(this, "Número de carnet no encontrado.");
}
}
public static void main(String[] args) {
SwingUtilities.invokeLater(() -> new SistemaDeTareasGUI().setVisible(true));
}
}
