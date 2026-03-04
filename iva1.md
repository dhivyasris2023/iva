import cv2
import numpy as np
from google.colab.patches import cv2_imshow

def build_t_pyramid(image, levels):
    pyramid = [image]
    for _ in range(levels - 1):
        image = cv2.pyrDown(image)
        pyramid.append(image)
    return pyramid

def main():
    image_path = "flower.webp"
    levels = 3

    original_image = cv2.imread(image_path)
    if original_image is None:
        print("Error: could not load the image")
        return

    t_pyramid = build_t_pyramid(original_image, levels)

    for i, level_image in enumerate(t_pyramid):
        print(f"Level {i}")
        cv2_imshow(level_image)

if __name__ == "__main__":
    main()
<img width="492" height="379" alt="Screenshot 2026-03-04 093111" src="https://github.com/user-attachments/assets/7ac9765a-c748-4daf-9b05-5e37d9bb8a78" />
