import cv2
import numpy as np
from google.colab.patches import cv2_imshow

# ---------- LOAD IMAGE ----------
image = cv2.imread("img.jpg")
if image is None:
    raise FileNotFoundError("Image not found. Check the path.")

# ---------- ROTATION ----------
def rotate_image(image, angle):
    h, w = image.shape[:2]
    matrix = cv2.getRotationMatrix2D((w / 2, h / 2), angle, 1)
    return cv2.warpAffine(image, matrix, (w, h))

rotated = rotate_image(image, 45)
cv2_imshow(rotated)


# ---------- SCALING ----------
def scale_image(image, scale_x, scale_y):
    return cv2.resize(image, None, fx=scale_x, fy=scale_y)

scaled = scale_image(image, 1.5, 1.5)
cv2_imshow(scaled)


# ---------- SKEW ----------
def skew_image(image, skew_x, skew_y):
    h, w = image.shape[:2]
    skew_matrix = np.float32([
        [1, skew_x, 0],
        [skew_y, 1, 0]
    ])
    return cv2.warpAffine(image, skew_matrix, (w, h))

skewed = skew_image(image, 0.2, 0.1)
cv2_imshow(skewed)


# ---------- AFFINE TRANSFORM ----------
def affine_transform(image, pts_src, pts_dst):
    matrix = cv2.getAffineTransform(pts_src, pts_dst)
    return cv2.warpAffine(image, matrix, (image.shape[1], image.shape[0]))

src_points = np.float32([[50, 50], [200, 50], [50, 200]])
dst_points = np.float32([[10, 100], [200, 50], [100, 250]])

affine_transformed = affine_transform(image, src_points, dst_points)
cv2_imshow(affine_transformed)


# ---------- PERSPECTIVE (BILINEAR) TRANSFORM ----------
def bilinear_transform(image, pts_src, pts_dst):
    matrix = cv2.getPerspectiveTransform(pts_src, pts_dst)
    return cv2.warpPerspective(
        image, matrix, (image.shape[1], image.shape[0])
    )

src_points = np.float32([
    [56, 65],
    [368, 52],
    [28, 387],
    [389, 390]
])
dst_points = np.float32([
    [0, 0],
    [300, 0],
    [0, 300],
    [300, 300]
])

bilinear_transformed = bilinear_transform(image, src_points, dst_points)
cv2_imshow(bilinear_transformed)
<img width="331" height="602" alt="Screenshot 2026-03-04 093337" src="https://github.com/user-attachments/assets/a8fbd61e-3124-44b0-ad19-b9270ff4d14b" />
