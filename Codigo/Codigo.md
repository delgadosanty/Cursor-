package com.saludo;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Alert;
import javafx.scene.control.Button;
import javafx.scene.control.ComboBox;
import javafx.scene.control.Label;
import javafx.scene.control.RadioButton;
import javafx.scene.control.TextField;
import javafx.scene.control.ToggleGroup;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.StackPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class SaludoApp extends Application {

    private StackPane root;

    @Override
    public void start(Stage stage) {
        root = new StackPane();
        root.getStyleClass().add("root-pane");
        mostrarInicio();

        Scene scene = new Scene(root, 520, 420);
        scene.getStylesheets().add(getClass().getResource("/styles.css").toExternalForm());

        stage.setTitle("Saludo al estudiante");
        stage.setScene(scene);
        stage.setMinWidth(460);
        stage.setMinHeight(380);
        stage.show();
    }

    private void mostrarInicio() {
        Label titulo = new Label("Bienvenido");
        titulo.getStyleClass().add("titulo");

        Label instruccion = new Label("Haz clic para solicitar un saludo personalizado.");
        instruccion.getStyleClass().add("subtitulo");

        Button solicitar = new Button("Solicitar saludo");
        solicitar.getStyleClass().add("boton-principal");
        solicitar.setOnAction(e -> mostrarFormulario());

        VBox caja = new VBox(16, titulo, instruccion, solicitar);
        caja.setAlignment(Pos.CENTER);
        caja.getStyleClass().add("tarjeta");

        reemplazarContenido(caja);
    }

    private void mostrarFormulario() {
        Label titulo = new Label("Datos del estudiante");
        titulo.getStyleClass().add("titulo");

        Label pedido = new Label("Ingresa tu nombre, edad y hora (AM/PM).");
        pedido.getStyleClass().add("subtitulo");

        TextField nombre = campo("Nombre");
        TextField edad = campo("Edad");

        ComboBox<Integer> hora = new ComboBox<>();
        for (int h = 1; h <= 12; h++) {
            hora.getItems().add(h);
        }
        hora.getSelectionModel().select(Integer.valueOf(8));
        hora.setPrefWidth(90);

        ToggleGroup periodo = new ToggleGroup();
        RadioButton am = new RadioButton("AM");
        RadioButton pm = new RadioButton("PM");
        am.setToggleGroup(periodo);
        pm.setToggleGroup(periodo);
        am.setSelected(true);

        HBox horaCaja = new HBox(12, new Label("Hora:"), hora, am, pm);
        horaCaja.setAlignment(Pos.CENTER_LEFT);

        Button saludar = new Button("Saludar");
        saludar.getStyleClass().add("boton-principal");
        saludar.setOnAction(e -> {
            String nombreValor = nombre.getText() == null ? "" : nombre.getText().trim();
            String edadTexto = edad.getText() == null ? "" : edad.getText().trim();

            if (nombreValor.isEmpty()) {
                avisar("Falta el nombre", "Escribe tu nombre para continuar.");
                return;
            }

            int edadValor;
            try {
                edadValor = Integer.parseInt(edadTexto);
            } catch (NumberFormatException ex) {
                avisar("Edad inválida", "Escribe la edad como un número entero.");
                return;
            }
            if (edadValor < 1 || edadValor > 120) {
                avisar("Edad inválida", "La edad debe estar entre 1 y 120.");
                return;
            }

            Integer horaValor = hora.getValue();
            boolean esAm = am.isSelected();
            mostrarSaludo(nombreValor, edadValor, horaValor, esAm);
        });

        Button volver = new Button("Volver");
        volver.getStyleClass().add("boton-secundario");
        volver.setOnAction(e -> mostrarInicio());

        HBox acciones = new HBox(12, volver, saludar);
        acciones.setAlignment(Pos.CENTER);

        VBox caja = new VBox(14, titulo, pedido, nombre, edad, horaCaja, acciones);
        caja.setAlignment(Pos.CENTER);
        caja.getStyleClass().add("tarjeta");

        reemplazarContenido(caja);
    }

    private void mostrarSaludo(String nombre, int edad, int hora, boolean esAm) {
        String momento;
        if (esAm) {
            momento = hora < 12 ? "Buenos días" : "Buenas noches";
        } else if (hora == 12 || hora < 6) {
            momento = "Buenas tardes";
        } else {
            momento = "Buenas noches";
        }

        String periodo = esAm ? "AM" : "PM";
        Label titulo = new Label(momento + ", " + nombre);
        titulo.getStyleClass().add("titulo");

        Label detalle = new Label(
                "Tienes " + edad + " años. Son las " + hora + " " + periodo + ".");
        detalle.getStyleClass().add("subtitulo");
        detalle.setWrapText(true);

        Button otro = new Button("Nuevo saludo");
        otro.getStyleClass().add("boton-principal");
        otro.setOnAction(e -> mostrarFormulario());

        Button inicio = new Button("Inicio");
        inicio.getStyleClass().add("boton-secundario");
        inicio.setOnAction(e -> mostrarInicio());

        HBox acciones = new HBox(12, inicio, otro);
        acciones.setAlignment(Pos.CENTER);

        VBox caja = new VBox(16, titulo, detalle, acciones);
        caja.setAlignment(Pos.CENTER);
        caja.getStyleClass().add("tarjeta");

        reemplazarContenido(caja);
    }

    private TextField campo(String placeholder) {
        TextField campo = new TextField();
        campo.setPromptText(placeholder);
        campo.setMaxWidth(280);
        return campo;
    }

    private void avisar(String titulo, String mensaje) {
        Alert alerta = new Alert(Alert.AlertType.WARNING);
        alerta.setTitle(titulo);
        alerta.setHeaderText(null);
        alerta.setContentText(mensaje);
        alerta.showAndWait();
    }

    private void reemplazarContenido(VBox contenido) {
        BorderPane marco = new BorderPane(contenido);
        marco.setPadding(new Insets(28));
        root.getChildren().setAll(marco);
    }

    public static void main(String[] args) {
        launch(args);
    }
}
