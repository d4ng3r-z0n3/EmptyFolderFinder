import sys
import os
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QLineEdit, QPushButton, QMessageBox, QSpinBox, QFileDialog

class EmptyFolderFinder(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("Empty Folder Finder")
        self.layout = QVBoxLayout()

        self.folder_label = QLabel("Folder Path:")
        self.folder_edit = QLineEdit()
        self.folder_button = QPushButton("Browse")
        self.folder_button.clicked.connect(self.select_folder)

        self.depth_label = QLabel("Maximum Depth:")
        self.depth_spinbox = QSpinBox()
        self.depth_spinbox.setMinimum(0)
        self.depth_spinbox.setMaximum(10)
        self.depth_spinbox.setValue(0)

        self.find_empty_folders_button = QPushButton("Find Empty Folders")
        self.find_empty_folders_button.clicked.connect(self.find_empty_folders)

        self.layout.addWidget(self.folder_label)
        self.layout.addWidget(self.folder_edit)
        self.layout.addWidget(self.folder_button)
        self.layout.addWidget(self.depth_label)
        self.layout.addWidget(self.depth_spinbox)
        self.layout.addWidget(self.find_empty_folders_button)

        self.setLayout(self.layout)

    def select_folder(self):
        folder_path = QFileDialog.getExistingDirectory(self, "Select Folder")
        self.folder_edit.setText(folder_path)

    def find_empty_folders(self):
        folder_path = self.folder_edit.text()
        max_depth = self.depth_spinbox.value()

        if not folder_path:
            QMessageBox.warning(self, "Error", "Please select a folder.")
            return

        try:
            empty_folders = []
            for root, dirs, files in os.walk(folder_path):
                current_depth = root.count(os.path.sep) - folder_path.count(os.path.sep)
                if max_depth > 0 and current_depth > max_depth:
                    continue
                if not dirs and not files:
                    empty_folders.append(root)
        except:
            QMessageBox.warning(self, "Error", "Failed to access the folder.")
            return

        if not empty_folders:
            QMessageBox.information(self, "Information", "No empty folders found.")
            return

        confirmation_message = "The following empty folders will be deleted:\n\n"
        confirmation_message += "\n".join(empty_folders) + "\n\n"
        confirmation_message += "Do you want to proceed?"

        msg_box = QMessageBox(self)
        msg_box.setWindowTitle("Confirmation")
        msg_box.setText(confirmation_message)
        msg_box.setIcon(QMessageBox.Question)
        msg_box.setStandardButtons(QMessageBox.Yes | QMessageBox.No)
        msg_box.setDefaultButton(QMessageBox.No)

        if msg_box.exec_() == QMessageBox.Yes:
            for folder in empty_folders:
                try:
                    os.rmdir(folder)
                except:
                    QMessageBox.warning(self, "Error", f"Failed to delete folder: {folder}")

            QMessageBox.information(self, "Information", "Empty folders deleted successfully.")
        else:
            QMessageBox.information(self, "Information", "Operation canceled.")


if __name__ == "__main__":
    app = QApplication(sys.argv)

    empty_folder_finder = EmptyFolderFinder()
    empty_folder_finder.show()

    sys.exit(app.exec_())
