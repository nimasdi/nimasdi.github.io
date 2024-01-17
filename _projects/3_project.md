---
layout: page
title: discrete math project
description: my discrete math project using qt
img: assets/img/dm.jpeg
tags: project math gui qt
importance: 3
category: work
---

This is my discrete math class project.
it's gui was created using pyQT.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/gui-qt.png" title="image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is how the gui looks
</div>

This is widget.py file for the project
{% raw %}

```python

import sys

from PySide6.QtWidgets import QApplication, QWidget , QLineEdit

from ui_form import Ui_Widget

from q1 import shortest_path
from q2 import shortest_path_time

class Widget(QWidget):
    def __init__(self, parent=None):
        super().__init__(parent)
        self.ui = Ui_Widget()
        self.ui.setupUi(self)

        self.ui.q1Button.clicked.connect(self.q1ButtonClicked)
        self.ui.q2Button.clicked.connect(self.q2ButtonClicked)

        self.ui.submitButton.clicked.connect(self.submitButtonClicked)

        self.ui.outputLabel.hide()
        self.ui.outputTextEdit.hide()

        self.ui.resetButton.clicked.connect(self.resetButtonClicked)


    def q1ButtonClicked(self):
        self.ui.lineEdit.setText("The least number of train stations")

    def q2ButtonClicked(self):
        self.ui.lineEdit.setText("The shortest path to the destination station")

    def submitButtonClicked(self):

        input_text = self.ui.lineEdit.text()

        if "least number of train stations" in input_text.lower():

            graph_input = self.ui.inputTextEdit.toPlainText()

            lines = graph_input.split('\n')
            graph = {}

            for line in lines[1:-1]:
                values = line.split()
                if len(values) >= 2:
                    u, v = values[:2]
                    if u not in graph:
                        graph[u] = []
                    if v not in graph:
                        graph[v] = []
                    graph[u].append(v)
                    graph[v].append(u)

            start_node = lines[-1].split()[0]

            result_distances = shortest_path(graph, start_node)

            for station, distance in result_distances.items():
                result_str = f"{station}: {distance}\n"
                self.ui.outputTextEdit.insertPlainText(result_str)



            self.ui.outputLabel.show()
            self.ui.outputTextEdit.show()

        elif "shortest path to the destination station" in input_text.lower():


            graph_input = self.ui.inputTextEdit.toPlainText()

            lines = graph_input.split('\n')
            graph = {}

            for line in lines[1:-2]:
                values = line.split()
                if len(values) >= 3:
                    station1, station2, distance = values[:3]

                    # Convert distance to float
                    distance = float(distance)

                    graph.setdefault(station1, []).append((station2, distance))
                    graph.setdefault(station2, []).append((station1, distance))

            # Extracting source and destination stations from the last two lines
            source, destination = lines[-2].split(), lines[-1].split()

            # Call the function for the second question
            time, path = shortest_path_time(len(graph), len(graph), list(graph.items()), graph, source[0], destination[0])


            result_str = f"Time: {time}\nPath: {' -> '.join(path)}\n"



            self.ui.outputTextEdit.insertPlainText(result_str)


            self.ui.outputLabel.show()
            self.ui.outputTextEdit.show()



        self.setFixedSize(800, 600)

    def resetButtonClicked(self):
        widget.setFixedSize(800 , 300)



if __name__ == "__main__":
    app = QApplication(sys.argv)
    widget = Widget()
    widget.setFixedSize(800 , 300)


    widget.show()
    sys.exit(app.exec())


```

{% endraw %}
