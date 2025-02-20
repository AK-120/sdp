### mainactvity.java
``` java
package com.example.exp3;

import android.app.AlertDialog;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        findViewById(R.id.btnOpenCalculator).setOnClickListener(v -> showCalculatorDialog());
    }

    private void showCalculatorDialog() {
        View view = getLayoutInflater().inflate(R.layout.dialog_calculator, null);
        EditText etNum1 = view.findViewById(R.id.etNum1);
        EditText etNum2 = view.findViewById(R.id.etNum2);
        EditText etOperator = view.findViewById(R.id.etOperator);

        new AlertDialog.Builder(this)
                .setTitle("Simple Calculator")
                .setView(view)
                .setPositiveButton("Calculate", (dialog, which) -> {
                    String num1Str = etNum1.getText().toString().trim();
                    String num2Str = etNum2.getText().toString().trim();
                    String operator = etOperator.getText().toString().trim();

                    if (num1Str.isEmpty() || num2Str.isEmpty() || operator.isEmpty()) {
                        Toast.makeText(this, "Please enter all fields", Toast.LENGTH_LONG).show();
                        return;
                    }

                    if (operator.length() != 1 || "+-*/".indexOf(operator) == -1) {
                        Toast.makeText(this, "Enter a valid operator (+, -, *, /)", Toast.LENGTH_LONG).show();
                        return;
                    }

                    try {
                        double num1 = Double.parseDouble(num1Str);
                        double num2 = Double.parseDouble(num2Str);
                        double result = performCalculation(num1, num2, operator);
                        showResultDialog(result);
                    } catch (NumberFormatException e) {
                        Toast.makeText(this, "Invalid number format!", Toast.LENGTH_LONG).show();
                    }
                })
                .setNegativeButton("Cancel", null)
                .show();
    }

    private double performCalculation(double num1, double num2, String operator) {
        switch (operator) {
            case "+": return num1 + num2;
            case "-": return num1 - num2;
            case "*": return num1 * num2;
            case "/":
                if (num2 == 0) {
                    Toast.makeText(this, "Cannot divide by zero!", Toast.LENGTH_LONG).show();
                    return Double.NaN; // Return NaN if division by zero
                }
                return num1 / num2;
            default:
                return 0;
        }
    }

    private void showResultDialog(double result) {
        new AlertDialog.Builder(this)
                .setTitle("Result")
                .setMessage("Result: " + result)
                .setPositiveButton("OK", null)
                .show();
    }
}
```
### res/layout/dialog_calculator.xml
``` xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/etNum1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter first number"
        android:inputType="numberDecimal" />

    <EditText
        android:id="@+id/etNum2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter second number"
        android:inputType="numberDecimal" />

    <EditText
        android:id="@+id/etOperator"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter operator (+, -, *, /)"
        android:inputType="text" />

</LinearLayout>
```
### activity_main.xml
``` xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnOpenCalculator"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Open Calculator" />
</LinearLayout>
```
