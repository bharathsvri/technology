# JavaFX Basics

## What is JavaFX?
JavaFX is a platform for creating rich client applications with Java.

## Basic Application
```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class HelloJavaFX extends Application {
    @Override
    public void start(Stage primaryStage) {
        Button btn = new Button("Click Me!");
        btn.setOnAction(e -> System.out.println("Button clicked"));
        
        StackPane root = new StackPane();
        root.getChildren().add(btn);
        
        Scene scene = new Scene(root, 300, 200);
        primaryStage.setTitle("Hello JavaFX");
        primaryStage.setScene(scene);
        primaryStage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

## Layouts

### VBox (Vertical)
```java
import javafx.scene.layout.VBox;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;

VBox vbox = new VBox(10); // 10px spacing
vbox.getChildren().addAll(
    new Label("Name:"),
    new TextField(),
    new Label("Email:"),
    new TextField()
);
```

### HBox (Horizontal)
```java
import javafx.scene.layout.HBox;
import javafx.scene.control.Button;

HBox hbox = new HBox(10);
hbox.getChildren().addAll(
    new Button("OK"),
    new Button("Cancel")
);
```

### BorderPane
```java
import javafx.scene.layout.BorderPane;

BorderPane borderPane = new BorderPane();
borderPane.setTop(new Label("Top"));
borderPane.setCenter(new Label("Center"));
borderPane.setBottom(new Label("Bottom"));
borderPane.setLeft(new Label("Left"));
borderPane.setRight(new Label("Right"));
```

## Controls

### TextField
```java
TextField textField = new TextField();
textField.setPromptText("Enter text");
String text = textField.getText();
```

### Button
```java
Button button = new Button("Click");
button.setOnAction(e -> {
    System.out.println("Button clicked");
});
```

### TableView
```java
import javafx.scene.control.TableView;
import javafx.scene.control.TableColumn;
import javafx.collections.FXCollections;

TableView<Person> table = new TableView<>();
TableColumn<Person, String> nameColumn = 
    new TableColumn<>("Name");
nameColumn.setCellValueFactory(
    data -> new SimpleStringProperty(data.getValue().getName()));
table.getColumns().add(nameColumn);
```

## Event Handling
```java
button.setOnAction(e -> {
    // Handle button click
});

textField.setOnKeyPressed(e -> {
    if (e.getCode() == KeyCode.ENTER) {
        // Handle Enter key
    }
});
```

## Best Practices
1. Use FXML for complex UIs
2. Separate UI from business logic
3. Use CSS for styling
4. Handle events properly
5. Use appropriate layouts
6. Make UI responsive
7. Test on different platforms
8. Follow JavaFX patterns

