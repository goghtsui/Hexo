title: Android之Matrix
date: 2016-02-19 15:11:37
categories: [Android]
tags: [Matrix]
---

> 原作者： Qiengo
> 源地址： [点击跳转](http://www.cnblogs.com/qiengo/archive/2012/06/30/2570874.html)


## Matrix的数学原理

在Android中，如果你用Matrix进行过图像处理，那么一定知道Matrix这个类。Android中的Matrix是一个3 x 3的矩阵，其内容如下：
![matrix_iamge_description][0]

Matrix的对图像的处理可分为四类基本变换：

- Translate - 平移变换
- Rotate - 旋转变换
- Scale - 缩放变换
- Skew - 错切变换


从字面上理解，矩阵中的MSCALE用于处理缩放变换，MSKEW用于处理错切变换，MTRANS用于处理平移变换，MPERSP用于处理透视变换。实际中当然不能完全按照字面上的说法去理解Matrix。同时，在Android的文档中，未见到用Matrix进行透视变换的相关说明，所以本文也不讨论这方面的问题。

针对每种变换，Android提供了pre、set和post三种操作方式。其中：

- set - 用于设置Matrix中的值。
- pre - 是先乘，因为矩阵的乘法不满足交换律，因此先乘、后乘必须要严格区分。先乘相当于矩阵运算中的右乘。
- post - 是后乘，因为矩阵的乘法不满足交换律，因此先乘、后乘必须要严格区分。后乘相当于矩阵运算中的左乘。

除平移变换(Translate)外，旋转变换(Rotate)、缩放变换(Scale)和错切变换(Skew)都可以围绕一个中心点来进行，如果不指定，在默认情况下是围绕(0, 0)来进行相应的变换的。


下面我们来看看四种变换的具体情形。由于所有的图形都是有点组成，因此我们只需要考察一个点相关变换即可。

 
### 一、 平移变换
---
假定有一个点的坐标是 ，将其移动到 ，再假定在x轴和y轴方向移动的大小分别为：
![translate][11]

如下图所示：
![translate_sample][12]

不难知道：
![translate_result][13]

如果用矩阵来表示的话，就可以写成：
![translate_tips][14]
 

### 二、 旋转变换
---

#### 2.1    围绕坐标原点旋转：

假定有一个点 ，相对坐标原点顺时针旋转后的情形，同时假定P点离坐标原点的距离为r，如下图：
![translate_tips][21]

那么，
![translate_tips][22]

如果用矩阵，就可以表示为：
![translate_tips][23]


#### 2.2    围绕某个点旋转

如果是围绕某个点顺时针旋转，那么可以用矩阵表示为：
![translate_tips][24]

可以化为：
![translate_tips][25]

很显然，

**1.**   
![translate_tips][26] 是将坐标原点移动到点后， 的新坐标。

**2.**     

![translate_tips][27] 是将上一步变换后的，围绕新的坐标原点顺时针旋转 。

**3.**     
![translate_tips][28] 经过上一步旋转变换后，再将坐标原点移回到原来的坐标原点。

所以，围绕某一点进行旋转变换，可以分成3个步骤，即首先将坐标原点移至该点，然后围绕新的坐标原点进行旋转变换，再然后将坐标原点移回到原先的坐标原点。

 

### 三、 缩放变换
---

理论上而言，一个点是不存在什么缩放变换的，但考虑到所有图像都是由点组成，因此，如果图像在x轴和y轴方向分别放大k1和k2倍的话，那么图像中的所有点的x坐标和y坐标均会分别放大k1和k2倍，即



用矩阵表示就是：
![translate_tips][31]

缩放变换比较好理解，就不多说了。
![translate_tips][32]

### 四、 错切变换
---

错切变换(skew)在数学上又称为Shear mapping(可译为“剪切变换”)或者Transvection(缩并)，它是一种比较特殊的线性变换。错切变换的效果就是让所有点的x坐标(或者y坐标)保持不变，而对应的y坐标(或者x坐标)则按比例发生平移，且平移的大小和该点到x轴(或y轴)的垂直距离成正比。错切变换，属于等面积变换，即一个形状在错切变换的前后，其面积是相等的。

比如下图，各点的y坐标保持不变，但其x坐标则按比例发生了平移。这种情况将水平错切。
![translate_tips][41]


下图各点的x坐标保持不变，但其y坐标则按比例发生了平移。这种情况叫垂直错切。
![translate_tips][42]
 
假定一个点经过错切变换后得到，对于水平错切而言，应该有如下关系：
![translate_tips][43]

用矩阵表示就是：
![translate_tips][44]

扩展到3 x 3的矩阵就是下面这样的形式：
![translate_tips][45]
 
同理，对于垂直错切，可以有：
![translate_tips][46]

在数学上严格的错切变换就是上面这样的。在Android中除了有上面说到的情况外，还可以同时进行水平、垂直错切，那么形式上就是：
![translate_tips][47]


### 五、 对称变换
---

除了上面讲到的4中基本变换外，事实上，我们还可以利用Matrix，进行对称变换。所谓对称变换，就是经过变化后的图像和原图像是关于某个对称轴是对称的。比如，某点 经过对称变换后得到，

如果对称轴是x轴，难么，
![translate_tips][51]

用矩阵表示就是：
![translate_tips][52]

如果对称轴是y轴，那么，
![translate_tips][53]

用矩阵表示就是：
![translate_tips][54]

如果对称轴是y = x，如图：
![translate_tips][55]

那么，
![translate_tips][56]

很容易可以解得：
![translate_tips][57]

用矩阵表示就是：
![translate_tips][58]

同样的道理，如果对称轴是y = -x，那么用矩阵表示就是：
![translate_tips][59]
 
特殊地，如果对称轴是y = kx，如下图：
![translate_tips][60]

那么，
![translate_tips][61]

很容易可解得：
![translate_tips][62]

用矩阵表示就是：
![translate_tips][63]

当k = 0时，即y = 0，也就是对称轴为x轴的情况；当k趋于无穷大时，即x = 0，也就是对称轴为y轴的情况；当k =1时，即y = x，也就是对称轴为y = x的情况；当k = -1时，即y = -x，也就是对称轴为y = -x的情况。不难验证，这和我们前面说到的4中具体情况是相吻合的。

如果对称轴是y = kx + b这样的情况，只需要在上面的基础上增加两次平移变换即可，即先将坐标原点移动到(0, b)，然后做上面的关于y = kx的对称变换，再然后将坐标原点移回到原来的坐标原点即可。用矩阵表示大致是这样的：
![translate_tips][64]

需要特别注意：在实际编程中，我们知道屏幕的y坐标的正向和数学中y坐标的正向刚好是相反的，所以在数学上y = x和屏幕上的y = -x才是真正的同一个东西，反之亦然。也就是说，如果要使图片在屏幕上看起来像按照数学意义上y = x对称，那么需使用这种转换：
![translate_tips][65]

要使图片在屏幕上看起来像按照数学意义上y = -x对称，那么需使用这种转换：
![translate_tips][66]

关于对称轴为y = kx 或y = kx + b的情况，同样需要考虑这方面的问题。



## 第二部分 代码验证

在第一部分中讲到的各种图像变换的验证代码如下，一共列出了10种情况。如果要验证其中的某一种情况，只需将相应的代码反注释即可。试验中用到的图片：
![][100]
其尺寸为162 x 251。

每种变换的结果，请见代码之后的说明。

``` java
package com.pat.testtransformmatrix;  
  
import android.app.Activity;  
import android.content.Context;  
import android.graphics.Bitmap;  
import android.graphics.BitmapFactory;  
import android.graphics.Canvas;  
import android.graphics.Matrix;  
import android.os.Bundle;  
import android.util.Log;  
import android.view.MotionEvent;  
import android.view.View;  
import android.view.Window;  
import android.view.WindowManager;  
import android.view.View.OnTouchListener;  
import android.widget.ImageView;  
  
public class TestTransformMatrixActivity extends Activity  
implements  
OnTouchListener  
{  
    private TransformMatrixView view;  
    @Override  
    public void onCreate(Bundle savedInstanceState)  
    {  
        super.onCreate(savedInstanceState);  
        requestWindowFeature(Window.FEATURE_NO_TITLE);  
        this.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);  
  
        view = new TransformMatrixView(this);  
        view.setScaleType(ImageView.ScaleType.MATRIX);  
        view.setOnTouchListener(this);  
          
        setContentView(view);  
    }  
      
    class TransformMatrixView extends ImageView  
    {  
        private Bitmap bitmap;  
        private Matrix matrix;  
        public TransformMatrixView(Context context)  
        {  
            super(context);  
            bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.sophie);  
            matrix = new Matrix();  
        }  
  
        @Override  
        protected void onDraw(Canvas canvas)  
        {  
            // 画出原图像  
            canvas.drawBitmap(bitmap, 0, 0, null);  
            // 画出变换后的图像  
            canvas.drawBitmap(bitmap, matrix, null);  
            super.onDraw(canvas);  
        }  
  
        @Override  
        public void setImageMatrix(Matrix matrix)  
        {  
            this.matrix.set(matrix);  
            super.setImageMatrix(matrix);  
        }  
          
        public Bitmap getImageBitmap()  
        {  
            return bitmap;  
        }  
    }  
  
    public boolean onTouch(View v, MotionEvent e)  
    {  
        if(e.getAction() == MotionEvent.ACTION_UP)  
        {  
            Matrix matrix = new Matrix();  
            // 输出图像的宽度和高度(162 x 251)  
            Log.e("TestTransformMatrixActivity", "image size: width x height = " +  view.getImageBitmap().getWidth() + " x " + view.getImageBitmap().getHeight());  
            // 1. 平移  
            matrix.postTranslate(view.getImageBitmap().getWidth(), view.getImageBitmap().getHeight());  
            // 在x方向平移view.getImageBitmap().getWidth()，在y轴方向view.getImageBitmap().getHeight()  
            view.setImageMatrix(matrix);  
              
            // 下面的代码是为了查看matrix中的元素  
            float[] matrixValues = new float[9];  
            matrix.getValues(matrixValues);  
            for(int i = 0; i < 3; ++i)  
            {  
                String temp = new String();  
                for(int j = 0; j < 3; ++j)  
                {  
                    temp += matrixValues[3 * i + j ] + "\t";  
                }  
                Log.e("TestTransformMatrixActivity", temp);  
            }  
              
  
//          // 2. 旋转(围绕图像的中心点)  
//          matrix.setRotate(45f, view.getImageBitmap().getWidth() / 2f, view.getImageBitmap().getHeight() / 2f);  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠  
//          matrix.postTranslate(view.getImageBitmap().getWidth() * 1.5f, 0f);  
//          view.setImageMatrix(matrix);  
//  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
              
              
//          // 3. 旋转(围绕坐标原点) + 平移(效果同2)  
//          matrix.setRotate(45f);  
//          matrix.preTranslate(-1f * view.getImageBitmap().getWidth() / 2f, -1f * view.getImageBitmap().getHeight() / 2f);  
//          matrix.postTranslate((float)view.getImageBitmap().getWidth() / 2f, (float)view.getImageBitmap().getHeight() / 2f);  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠  
//          matrix.postTranslate((float)view.getImageBitmap().getWidth() * 1.5f, 0f);  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }             
              
//          // 4. 缩放  
//          matrix.setScale(2f, 2f);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠  
//          matrix.postTranslate(view.getImageBitmap().getWidth(), view.getImageBitmap().getHeight());  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
  
              
//          // 5. 错切 - 水平  
//          matrix.setSkew(0.5f, 0f);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠           
//          matrix.postTranslate(view.getImageBitmap().getWidth(), 0f);  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
              
//          // 6. 错切 - 垂直  
//          matrix.setSkew(0f, 0.5f);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠               
//          matrix.postTranslate(0f, view.getImageBitmap().getHeight());  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }             
              
//          7. 错切 - 水平 + 垂直  
//          matrix.setSkew(0.5f, 0.5f);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠               
//          matrix.postTranslate(0f, view.getImageBitmap().getHeight());  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
              
//          // 8. 对称 (水平对称)  
//          float matrix_values[] = {1f, 0f, 0f, 0f, -1f, 0f, 0f, 0f, 1f};  
//          matrix.setValues(matrix_values);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠   
//          matrix.postTranslate(0f, view.getImageBitmap().getHeight() * 2f);  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }             
              
//          // 9. 对称 - 垂直  
//          float matrix_values[] = {-1f, 0f, 0f, 0f, 1f, 0f, 0f, 0f, 1f};  
//          matrix.setValues(matrix_values);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }     
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠   
//          matrix.postTranslate(view.getImageBitmap().getWidth() * 2f, 0f);  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
  
              
//          // 10. 对称(对称轴为直线y = x)  
//          float matrix_values[] = {0f, -1f, 0f, -1f, 0f, 0f, 0f, 0f, 1f};  
//          matrix.setValues(matrix_values);  
//          // 下面的代码是为了查看matrix中的元素  
//          float[] matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
//            
//          // 做下面的平移变换，纯粹是为了让变换后的图像和原图像不重叠               
//          matrix.postTranslate(view.getImageBitmap().getHeight() + view.getImageBitmap().getWidth(),   
//                  view.getImageBitmap().getHeight() + view.getImageBitmap().getWidth());  
//          view.setImageMatrix(matrix);  
//            
//          // 下面的代码是为了查看matrix中的元素  
//          matrixValues = new float[9];  
//          matrix.getValues(matrixValues);  
//          for(int i = 0; i < 3; ++i)  
//          {  
//              String temp = new String();  
//              for(int j = 0; j < 3; ++j)  
//              {  
//                  temp += matrixValues[3 * i + j ] + "\t";  
//              }  
//              Log.e("TestTransformMatrixActivity", temp);  
//          }  
              
            view.invalidate();  
        }  
        return true;  
    }  
}  
```

下面给出上述代码中，各种变换的具体结果及其对应的相关变换矩阵

### 1.平移
![][101]

输出的结果：
![][102]

请对照第一部分中的“一、平移变换”所讲的情形，考察上述矩阵的正确性。


### 2.旋转(围绕图像的中心点)
---
![][201]

输出的结果：
![][202]

它实际上是
``` java
matrix.setRotate(45f,view.getImageBitmap().getWidth() / 2f, view.getImageBitmap().getHeight() / 2f);
matrix.postTranslate(view.getImageBitmap().getWidth()* 1.5f, 0f);
```

这两条语句综合作用的结果。根据第一部分中“二、旋转变换”里面关于围绕某点旋转的公式，
``` java
matrix.setRotate(45f,view.getImageBitmap().getWidth() / 2f, view.getImageBitmap().getHeight() / 2f);
```

所产生的转换矩阵就是：
![][203]

而
``` java
matrix.postTranslate(view.getImageBitmap().getWidth()* 1.5f, 0f);
``` 
的意思就是在上述矩阵的左边再乘以下面的矩阵：
![][204]

关于post是左乘这一点，我们在前面的理论部分曾经提及过，后面我们还会专门讨论这个问题。
所以它实际上就是：
![][205]

出去计算上的精度误差，我们可以看到我们计算出来的结果，和程序直接输出的结果是一致的。

 
### 3.旋转(围绕坐标原点旋转，在加上两次平移，效果同2)
---
![][301]

根据第一部分中“二、旋转变换”里面关于围绕某点旋转的解释，不难知道：
``` java
matrix.setRotate(45f,view.getImageBitmap().getWidth() / 2f, view.getImageBitmap().getHeight() / 2f);
```
等价于
``` java
matrix.setRotate(45f);
matrix.preTranslate(-1f* view.getImageBitmap().getWidth() / 2f, -1f *view.getImageBitmap().getHeight() / 2f);
matrix.postTranslate((float)view.getImageBitmap().getWidth()/ 2f, (float)view.getImageBitmap().getHeight() / 2f);
```

其中`matrix.setRotate(45f)`对应的矩阵是：
![][302]

``` java
matrix.preTranslate(-1f* view.getImageBitmap().getWidth() / 2f, -1f * view.getImageBitmap().getHeight()/ 2f)
```
对应的矩阵是：
![][303]

由于是preTranslate，是先乘，也就是右乘，即它应该出现在`matrix.setRotate(45f)`所对应矩阵的右侧。

``` java
matrix.postTranslate((float)view.getImageBitmap().getWidth()/ 2f, (float)view.getImageBitmap().getHeight() / 2f)
```
对应的矩阵是：
![][304]

这次由于是postTranslate，是后乘，也就是左乘，即它应该出现在`matrix.setRotate(45f)`所对应矩阵的左侧。

所以综合起来，
``` java
matrix.setRotate(45f);
matrix.preTranslate(-1f* view.getImageBitmap().getWidth() / 2f, -1f *view.getImageBitmap().getHeight() / 2f);
matrix.postTranslate((float)view.getImageBitmap().getWidth()/ 2f, (float)view.getImageBitmap().getHeight() / 2f);
```
对应的矩阵就是：
![][305]

这和下面这个矩阵(围绕图像中心顺时针旋转45度)其实是一样的：
![][306]

因此，此处变换后的图像和2中变换后的图像时一样的。
![][307]

### 4.缩放变换
---
![][401]

程序所输出的两个矩阵分别是：
![][402]

其中第二个矩阵，其实是下面两个矩阵相乘的结果：
![][403]

大家可以对照第一部分中的“三、缩放变换”和“一、平移变换”说法，自行验证结果。


### 5.错切变换(水平错切)
---
![][501]

代码所输出的两个矩阵分别是：
![][502]

其中，第二个矩阵其实是下面两个矩阵相乘的结果：
![][503]

大家可以对照第一部分中的“四、错切变换”和“一、平移变换”的相关说法，自行验证结果。


### 6.错切变换(垂直错切)
---
![][601]

代码所输出的两个矩阵分别是：
![][602]

其中，第二个矩阵其实是下面两个矩阵相乘的结果：
![][603]

大家可以对照第一部分中的“四、错切变换”和“一、平移变换”的相关说法，自行验证结果。


### 7.错切变换(水平+垂直错切)
---
![][701]

代码所输出的两个矩阵分别是：
![][702]

其中，后者是下面两个矩阵相乘的结果：
![][703]

大家可以对照第一部分中的“四、错切变换”和“一、平移变换”的相关说法，自行验证结果。


### 8.对称变换(水平对称)
---
![][801]

代码所输出的两个各矩阵分别是：
![][802]

其中，后者是下面两个矩阵相乘的结果：
![][803]
 
大家可以对照第一部分中的“五、对称变换”和“一、平移变换”的相关说法，自行验证结果。

 
### 9.对称变换(垂直对称)
---
![][901]

代码所输出的两个矩阵分别是：
![][902]

其中，后者是下面两个矩阵相乘的结果：
![][903]

大家可以对照第一部分中的“五、对称变换”和“一、平移变换”的相关说法，自行验证结果。


### 10.对称变换(对称轴为直线y = x)
---
![][103]

代码所输出的两个矩阵分别是：
![][104]

其中，后者是下面两个矩阵相乘的结果：
![][105]
 
大家可以对照第一部分中的“五、对称变换”和“一、平移变换”的相关说法，自行验证结果。

 
### 11.关于先乘和后乘的问题
---
由于矩阵的乘法运算不满足交换律，我们在前面曾经多次提及先乘、后乘的问题，即先乘就是矩阵运算中右乘，后乘就是矩阵运算中的左乘。其实先乘、后乘的概念是针对变换操作的时间先后而言的，左乘、右乘是针对矩阵运算的左右位置而言的。以第一部分“二、旋转变换”中围绕某点旋转的情况为例：
![][110]

越靠近原图像中像素的矩阵，越先乘，越远离原图像中像素的矩阵，越后乘。事实上，图像处理时，矩阵的运算是从右边往左边方向进行运算的。这就形成了越在右边的矩阵(右乘)，越先运算(先乘)，反之亦然。

当然，在实际中，如果首先指定了一个matrix，比如我们先setRotate()，即指定了上面变换矩阵中，中间的那个矩阵，那么后续的矩阵到底是pre还是post运算，都是相对这个中间矩阵而言的。





[0]: http://hi.csdn.net/attachment/201111/19/0_13217092330d9Q.gif

[11]: http://hi.csdn.net/attachment/201111/19/0_1321709352RQ75.gif
[12]: http://hi.csdn.net/attachment/201111/19/0_1321709520MmsS.gif
[13]: http://hi.csdn.net/attachment/201111/19/0_1321709527kmK6.gif
[14]: http://hi.csdn.net/attachment/201111/19/0_1321709536Otg4.gif


[21]: http://hi.csdn.net/attachment/201111/19/0_132170975189NC.gif
[22]: http://hi.csdn.net/attachment/201111/19/0_1321709797SBJW.gif
[23]: http://hi.csdn.net/attachment/201111/19/0_1321709849ZLVc.gif
[24]: http://hi.csdn.net/attachment/201111/19/0_13217100380220.gif
[25]: http://hi.csdn.net/attachment/201111/19/0_13217100952Vqv.gif
[26]: http://hi.csdn.net/attachment/201111/19/0_1321710153kurQ.gif
[27]: http://hi.csdn.net/attachment/201111/19/0_1321710301T9nf.gif
[28]: http://hi.csdn.net/attachment/201111/19/0_1321710398Z3Je.gif


[31]: http://hi.csdn.net/attachment/201111/19/0_1321710517pb9W.gif
[32]: http://hi.csdn.net/attachment/201111/19/0_1321710615riwr.gif


[41]: http://hi.csdn.net/attachment/201111/19/0_1321710625smm5.gif
[42]: http://hi.csdn.net/attachment/201111/19/0_1321710790633H.gif
[43]: http://hi.csdn.net/attachment/201111/19/0_1321710798y5L6.gif
[44]: http://hi.csdn.net/attachment/201111/19/0_13217108084B3T.gif
[45]: http://hi.csdn.net/attachment/201111/19/0_13217108954sms.gif
[46]: http://hi.csdn.net/attachment/201111/19/0_13217109074Nv2.gif
[47]: http://hi.csdn.net/attachment/201111/19/0_1321711018S31a.gif


[51]: http://hi.csdn.net/attachment/201111/19/0_1321711026LZ03.gif
[52]: http://hi.csdn.net/attachment/201111/19/0_1321711090fhGd.gif
[53]: http://hi.csdn.net/attachment/201111/19/0_1321711099Xhak.gif
[54]: http://hi.csdn.net/attachment/201111/19/0_1321711217oHNz.gif
[55]: http://hi.csdn.net/attachment/201111/19/0_1321711240gEeT.gif
[56]: http://hi.csdn.net/attachment/201111/19/0_1321711240gEeT.gif
[57]: http://hi.csdn.net/attachment/201111/19/0_1321711261E6xG.gif
[58]: http://hi.csdn.net/attachment/201111/19/0_132171128473YK.gif
[59]: http://hi.csdn.net/attachment/201111/19/0_1321711292jO01.gif
[60]: http://hi.csdn.net/attachment/201111/19/0_13217113506Hb8.gif
[61]: http://hi.csdn.net/attachment/201111/19/0_1321711502QQ7A.gif
[62]: http://hi.csdn.net/attachment/201111/19/0_1321711521GZlt.gif
[63]: http://hi.csdn.net/attachment/201111/19/0_1321711541FJA1.gif
[64]: http://hi.csdn.net/attachment/201111/19/0_1321711616I9SJ.gif
[65]: http://hi.csdn.net/attachment/201111/19/0_1321711292jO01.gif
[66]: http://hi.csdn.net/attachment/201111/19/0_132171128473YK.gif


[100]: http://hi.csdn.net/attachment/201111/19/0_13217122673338.gif


[101]: http://hi.csdn.net/attachment/201111/19/0_1321712352qQRu.gif
[102]: http://hi.csdn.net/attachment/201111/19/0_13217123565Wwz.gif

[200]: http://hi.csdn.net/attachment/201111/19/0_132171250556xp.gif
[201]: http://hi.csdn.net/attachment/201111/19/0_132171250556xp.gif
[202]: http://hi.csdn.net/attachment/201111/19/0_1321712512Yj1i.gif
[203]: http://hi.csdn.net/attachment/201111/19/0_1321712644I54M.gif
[204]: http://hi.csdn.net/attachment/201111/19/0_13217126508k4V.gif
[205]: http://hi.csdn.net/attachment/201111/19/0_13217126608wdT.gif


[301]: http://hi.csdn.net/attachment/201111/19/0_132171250556xp.gif
[302]: http://hi.csdn.net/attachment/201111/19/0_1321712949GjN7.gif
[303]: http://hi.csdn.net/attachment/201111/19/0_1321712956BNj8.gif
[304]: http://hi.csdn.net/attachment/201111/19/0_1321712963iNO1.gif
[305]: http://hi.csdn.net/attachment/201111/19/0_1321713055HOOt.gif
[306]: http://hi.csdn.net/attachment/201111/19/0_1321713100VIOz.gif


[401]: http://hi.csdn.net/attachment/201111/19/0_1321713185yKS7.gif
[402]: http://hi.csdn.net/attachment/201111/19/0_13217131941R24.gif
[403]: http://hi.csdn.net/attachment/201111/19/0_1321713201VRxs.gif


[501]: http://hi.csdn.net/attachment/201111/19/0_132171330766G0.gif
[502]: http://hi.csdn.net/attachment/201111/19/0_1321713314Dk69.gif
[503]: http://hi.csdn.net/attachment/201111/19/0_1321713322PeML.gif


[601]: http://hi.csdn.net/attachment/201111/19/0_1321713502Akg2.gif
[602]: http://hi.csdn.net/attachment/201111/19/0_1321713509Hz7p.gif
[603]: http://hi.csdn.net/attachment/201111/19/0_1321713516TUvx.gif


[701]: http://hi.csdn.net/attachment/201111/19/0_1321713655Qsij.gif
[702]: http://hi.csdn.net/attachment/201111/19/0_13217136824kKR.gif
[703]: http://hi.csdn.net/attachment/201111/19/0_13217136931vl4.gif


[801]: http://hi.csdn.net/attachment/201111/19/0_13217141232rJI.gif
[802]: http://hi.csdn.net/attachment/201111/19/0_1321714133P0ha.gif
[803]: http://hi.csdn.net/attachment/201111/19/0_13217141417Bi3.gif


[901]: http://hi.csdn.net/attachment/201111/19/0_1321714261NN3V.gif
[902]: http://hi.csdn.net/attachment/201111/19/0_1321714269Kqs4.gif
[903]: http://hi.csdn.net/attachment/201111/19/0_1321714276ai2f.gif


[103]: http://hi.csdn.net/attachment/201111/19/0_13217144950030.gif
[104]: http://hi.csdn.net/attachment/201111/19/0_13217145038hN8.gif
[105]: http://hi.csdn.net/attachment/201111/19/0_1321714509sBN4.gif


[110]: http://hi.csdn.net/attachment/201111/19/0_1321714645396z.gif








