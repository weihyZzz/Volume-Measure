# Volume Measure

- **对物体体积信息的测量（最小化限制，误差范围在5毫米以内）**
- 超市或便利店商品装载所需的体积信息测量
![image](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/10ba33dd-c53a-46d3-a444-a24aae27e276/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221101T103242Z&X-Amz-Expires=86400&X-Amz-Signature=a3aee6a3a6b505f4c1d22ab545adefa729ab8c1f79c16664cfa04bd067fedb1a&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)


# 📝 Summary

在超市或便利店中，有效地陈列许多商品非常重要。因此，该项目能够立即测量收到的物体的体积信息，并以此信息找到最佳的陈列方法。此外，在快递的装卸过程中，该方法还可以在货车上有效地装载最多的商品，对于各种分销行业都有用处。由于不需要其他传感器或多台摄像机，仅使用单眼摄像头进行测量，因此在安装成本方面也将更具优势。

# ⭐️ Key Function

- **单眼相机**
    - 在没有其他硬件帮助的情况下，仅使用一台摄像机即可测量体积信息。
- **物体的形状种类**
    - 只能测量长方体和圆柱形状。
    - 在装卸系统中，由于大多数情况下使用长方体的箱子，因此预计不会有问题。
    - 在便利店和超市的情况下，由于水平和垂直的重要性更大，预计高度的问题不太重要。

# **Calibration.npz 文件创建方法**

[final_make_calibraton.py]이 为了正常运行，需要按照以下步骤进行。.

额外的信息请参考下面的链接。

[[OpenCV] 07-1. Camera Calibration — 深度学习 (Fresh-Learning) (tistory.com)](https://leechamin.tistory.com/345)

### 拿着棋盘的照片

[Camera Calibration Pattern Generator – calib.io](https://calib.io/pages/camera-calibration-pattern-generator)

# 使用方法

```python
import glob
from volume_measure import volumetric

main_path = "."
calibration_path = main_path + "/calibration" + "/cs_(8, 5)_rd_3_te_0.06_rs_4.npz"

hexahedron = glob.glob(main_path + "/hexahedron/*.jpg")
cylinder = glob.glob(main_path + "/cylinder/*.jpg")

images = hexahedron + cylinder

for fname in images:
    try:
        test = volumetric(fname, calibration_path)
        test.set_init()
        test.set_npz_values()

        # 1. 背景去除
        test.remove_background()

        # 2. 物体顶点检查
        test.find_vertex(draw=False)
        test.fix_vertex()

        test.trans_checker_stand_coor()
        test.set_transform_matrix()

        # 3. 获取宽度和高度
        test.measure_width_vertical()

        # 4. 获取高度
        test.measure_height(draw=True)

        # 在图像上绘制最佳信息
        test.draw_image()

        test.show_image(test.img, "Result Image")
        cv2.waitKey()

        cv2.destroyAllWindows()
    except:
        continue
```


# 📷 Screenshot(result) 
- 单位 : cm
- hexahedron
![image](https://drive.google.com/uc?export=view&id=16XEimDh3hfWV0f0Ds8dpFusU5i7LtNC8)
- cylinder
![image](https://drive.google.com/uc?export=view&id=1bW5UwwkYER18Mismg6gOChcaaZMl_qUW)

## ****References****

- **Rembg**:[https://github.com/danielgatis/rembg](https://github.com/danielgatis/rembg)
- **Opencv_Calibration**:[https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html](https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html)
- **ORB**:[https://docs.opencv.org/3.4/d1/d89/tutorial_py_orb.html](https://docs.opencv.org/3.4/d1/d89/tutorial_py_orb.html)

### **License**

Copyright (c) 2022 Yeardream-Power-4-team

Please contact me gomugomutree@gmail.com if you need help
