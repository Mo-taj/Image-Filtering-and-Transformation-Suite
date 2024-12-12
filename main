import tkinter as tk
from tkinter import filedialog, ttk
from PIL import Image, ImageTk
import cv2
import numpy as np


class ImageProcessingApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Image Processing Tool")

        # Frames
        self.control_frame = tk.Frame(self.root, padx=10, pady=10)
        self.control_frame.pack(side=tk.TOP, fill=tk.X)

        self.preview_frame = tk.Frame(self.root, padx=10, pady=10)
        self.preview_frame.pack(side=tk.TOP, fill=tk.BOTH, expand=True)

        self.operation_frame = tk.Frame(self.root, padx=10, pady=10)
        self.operation_frame.pack(side=tk.BOTTOM, fill=tk.X)

        # Buttons
        self.import_button = tk.Button(self.control_frame, text="Import", command=self.import_image)
        self.import_button.pack(side=tk.LEFT, padx=5)

        self.apply_button = tk.Button(self.control_frame, text="Apply", command=self.apply_operation)
        self.apply_button.pack(side=tk.LEFT, padx=5)

        self.reset_button = tk.Button(self.control_frame, text="Reset", command=self.reset_image)
        self.reset_button.pack(side=tk.LEFT, padx=5)

        self.exit_button = tk.Button(self.control_frame, text="Exit", command=self.root.quit)
        self.exit_button.pack(side=tk.RIGHT, padx=5)

        # Canvases
        self.original_canvas = tk.Label(self.preview_frame, text="No Image", bg="gray", width=50, height=25)
        self.original_canvas.grid(row=0, column=0, padx=10, pady=10, sticky="nsew")

        self.processed_canvas = tk.Label(self.preview_frame, text="No Image", bg="gray", width=50, height=25)
        self.processed_canvas.grid(row=0, column=1, padx=10, pady=10, sticky="nsew")

        # Configure grid resizing
        self.preview_frame.grid_rowconfigure(0, weight=1)
        self.preview_frame.grid_columnconfigure(0, weight=1)
        self.preview_frame.grid_columnconfigure(1, weight=1)

        # Dropdown for operations
        self.operations = ["Low-Pass Filter", "High-Pass Filter", "Edge Detection", "Grayscale", "Gaussian Blur"]
        self.selected_operation = tk.StringVar()
        self.selected_operation.set(self.operations[0])

        self.operation_menu = ttk.Combobox(self.operation_frame, textvariable=self.selected_operation, values=self.operations, state="readonly")
        self.operation_menu.pack(fill=tk.X, padx=5, pady=5)

        # Variables to store images
        self.original_image = None
        self.processed_image = None

    def import_image(self):
        file_path = filedialog.askopenfilename(filetypes=[("Image files", "*.jpg *.jpeg *.png *.bmp *.tiff")])
        if file_path:
            # Load the image using OpenCV
            self.original_image = cv2.cvtColor(cv2.imread(file_path), cv2.COLOR_BGR2RGB)
            self.processed_image = None  # Reset processed image

            # Display the original image
            self.show_image(self.original_image, self.original_canvas)

    def reset_image(self):
        if self.original_image is not None:
            self.processed_image = None
            self.show_image(self.original_image, self.original_canvas)
            self.show_image(None, self.processed_canvas)

    def apply_operation(self):
        if self.original_image is not None:
            operation = self.selected_operation.get()

            if operation == "Low-Pass Filter":
                kernel = np.ones((5, 5), np.float32) / 25
                self.processed_image = cv2.filter2D(self.original_image, -1, kernel)

            elif operation == "High-Pass Filter":
                kernel = np.array([[-1, -1, -1], [-1, 8, -1], [-1, -1, -1]])
                self.processed_image = cv2.filter2D(self.original_image, -1, kernel)

            elif operation == "Edge Detection":
                self.processed_image = cv2.Canny(self.original_image, 100, 200)

            elif operation == "Grayscale":
                self.processed_image = cv2.cvtColor(self.original_image, cv2.COLOR_RGB2GRAY)

            elif operation == "Gaussian Blur":
                self.processed_image = cv2.GaussianBlur(self.original_image, (5, 5), 0)

            self.show_image(self.processed_image, self.processed_canvas)

    def show_image(self, img, canvas):
        if img is not None:
            img = Image.fromarray(img)

            # Get canvas dimensions
            canvas_width = canvas.winfo_width()
            canvas_height = canvas.winfo_height()

            # Wait for canvas to update its size
            if canvas_width == 1 or canvas_height == 1:  # Happens on first render
                canvas.update_idletasks()
                canvas_width = canvas.winfo_width()
                canvas_height = canvas.winfo_height()

            # Resize the image to fit the canvas
            img_resized = img.resize((canvas_width, canvas_height), Image.Resampling.LANCZOS)
            img_tk = ImageTk.PhotoImage(img_resized)

            # Update the canvas with the resized image
            canvas.configure(image=img_tk, text="")
            canvas.image = img_tk
        else:
            canvas.configure(image='', text="No Image")
            canvas.image = None


if __name__ == "__main__":
    root = tk.Tk()

    # Set the default window size
    root.geometry("800x600")

    app = ImageProcessingApp(root)
    root.mainloop()
