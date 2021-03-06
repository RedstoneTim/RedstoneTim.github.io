---
title: WCA ID Records
---
This is a small (not necessarily well coded) program which shows you the results of cubers with a specific WCA ID year. It requires Java to run and uses JavaFX (so you don't need to and actually can't use the console).<br>
There also is a <a href="https://www.speedsolving.com/threads/odd-wca-stats-stats-request-thread.26121/page-250#post-1382600" target="_blank">SpeedSolving Forums post</a> for this.<br>
<a href="/assets/files/WCA_ID_Records.jar" download="WCA ID Records.jar">Click here to download</a>, the following is the source code (fits all into one file):<br>
<br>

{% highlight java %}
import java.io.BufferedInputStream;
import java.net.URL;
import java.text.NumberFormat;
import java.text.ParsePosition;
import java.util.Scanner;
import java.util.regex.Pattern;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.CheckBox;
import javafx.scene.control.ComboBox;
import javafx.scene.control.TextArea;
import javafx.scene.control.TextField;
import javafx.scene.control.TextFormatter;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class Main extends Application {
	public static final String WCA_EXPORT = "https://www.worldcubeassociation.org/results/misc/WCA_export.tsv.zip";
	public static final String SINGLES_RANK_FILE = "WCA_export_RanksSingle.tsv";
	public static final String AVERAGE_RANK_FILE = "WCA_export_RanksAverage.tsv";
	public static final Pattern WCA_ID = Pattern.compile("^\\d\\d\\d\\d+\\w\\w\\w\\w\\d+");
	public static final Pattern WCA_ID_YEAR = Pattern.compile("^\\d\\d\\d\\d+(?=\\w)");
	public static final String[] EVENTS = { "333", "222", "444", "555", "666", "777", "333bf", "333fm", "333oh",
			"clock", "minx", "pyram", "skewb", "sq1", "444bf", "555bf", "333mbf" };
	public static final NumberFormat INTEGER_FORMAT = NumberFormat.getIntegerInstance();

	public static void main(String[] args) {
		Application.launch(args);
	}

	@Override
	public void start(Stage window) throws Exception {
		TextArea output = new TextArea(
				"Enter what the program should search for below and then press the start button.");
		output.setEditable(false);

		TextField year = new TextField();
		year.setPromptText("WCA ID year");
		year.setTextFormatter(getIntegerFormatter());

		TextField resultCount = new TextField();
		resultCount.setPromptText("Persons to show");
		resultCount.setTextFormatter(getIntegerFormatter());

		CheckBox singleOrAverage = new CheckBox("Single (otherwise average)");
		singleOrAverage.setAllowIndeterminate(false);
		singleOrAverage.setSelected(true);

		ComboBox<String> eventName = new ComboBox<>();
		eventName.getItems().addAll(EVENTS);
		eventName.setEditable(true);
		eventName.getSelectionModel().selectFirst();

		Button startButton = new Button("Start");
		startButton.setOnAction(event -> {
			try {
				collectData(output, singleOrAverage.isSelected(), Integer.parseInt(resultCount.getText()),
						Integer.parseInt(year.getText()), eventName.getSelectionModel().getSelectedItem());
			} catch (NumberFormatException e) {
				output.setText(
						"Please enter a number for " + year.getPromptText() + " and " + resultCount.getPromptText());
			}
		});

		window.setHeight(500);
		window.setWidth(800);
		window.setTitle("WCA ID Records");
		window.setScene(new Scene(new VBox(output, startButton, year, resultCount, singleOrAverage, eventName)));
		window.show();
	}

	public void collectData(TextArea output, boolean singles, int resultCount, int year, String eventName) {
		new Thread(() -> {
			output.setText("Downloading data...");
			try (ZipInputStream inputStream = new ZipInputStream(
					new BufferedInputStream(new URL(WCA_EXPORT).openStream()))) {
				for (ZipEntry entry = inputStream.getNextEntry(); entry != null; entry = inputStream.getNextEntry()) {
					if ((singles ? SINGLES_RANK_FILE : AVERAGE_RANK_FILE).equals(entry.getName())) {
						output.setText("Successfully downloaded files.\nNow processing data...");
						Scanner scanner = new Scanner(inputStream);
						int resultCountCopy = resultCount;
						StringBuilder result = new StringBuilder();
						result.append(year).append(" WCA ID ").append(eventName)
								.append(singles ? " single" : " average").append(" records");
						while ((resultCountCopy > 0) && scanner.hasNextLine()) {
							String line = scanner.nextLine();
							if (line.startsWith(String.valueOf(year))) {
								String[] data = line.split("\t");
								if (eventName.equals(data[1])) {
									result.append("\n").append(resultCount - resultCountCopy + 1).append(". ")
											.append(data[0]).append(": ").append(data[2]);
									resultCountCopy--;
								}
							}
						}
						output.setText(result.toString());
						scanner.close();
						return;
					}
				}
				output.setText("Could not find the correct file");
			} catch (Exception e) {
				e.printStackTrace();
				output.setText("Caught error: " + e.getMessage());
			}
		}).start();
	}

	public TextFormatter<String> getIntegerFormatter() {
		return new TextFormatter<>(c -> {
			if (c.getControlNewText().isEmpty()) {
				return c;
			}
			ParsePosition parsePosition = new ParsePosition(0);
			return (INTEGER_FORMAT.parse(c.getControlNewText(), parsePosition) == null
					|| parsePosition.getIndex() < c.getControlNewText().length()) ? null : c;
		});
	}
}
{% endhighlight %}