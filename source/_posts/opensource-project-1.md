---
title: "opensource project 1"
layout: post
date: 2015-04-10
tags:
- opensource
categories:
- android
---
## 1.WaveView
**核心原理：**
```java
//1.画线 y=Asin(ωx+φ)+k
mAboveWavePath.moveTo(left, bottom);
for (float x = 0; x <= mMaxRight; x += X_SPACE) {
    y = (float) (mWaveHeight * Math.sin(omega * x + mAboveOffset) + mWaveHeight);
    mAboveWavePath.lineTo(x, y);
}
mAboveWavePath.lineTo(right, bottom);
//2.另外在利用Handler不断地更新mAboveOffset.
```

## 2.AndroidSwipeLayout
**核心原理：**
```java
//底层View，上层View，使用 ViewDragHelper.processTouchEvent(ev)
```

## 3.[ParallexListView](https://github.com/Gnod/ParallaxListView)
**核心原理：**
```java
//这段代码不错，使用了差值，嘿嘿，收了！
public class ResetAnimimation extends Animation {
    int targetHeight;
    int originalHeight;
    int extraHeight;
    View mView;
    protected ResetAnimimation(View view, int targetHeight) {
        this.mView = view;
        this.targetHeight = targetHeight;
        originalHeight = view.getHeight();
        extraHeight = this.targetHeight - originalHeight;
    }
    @Override
    protected void applyTransformation(float interpolatedTime,
                                       Transformation t) {
        int newHeight;
        newHeight = (int) (targetHeight - extraHeight * (1 - interpolatedTime));
        mView.getLayoutParams().height = newHeight;
        mView.requestLayout();
    }
}
//2.恢复高度的时候自动 mView.startAnimation(new ResetAnimation()) 就行了，不错！
//3.暂时未发现特别之处，就是继承了 ListView，然后监听AbsListView.OnScrollListener的这两个方法:
protected void onScrollChanged(int l, int t, int oldl, int oldt),
protected boolean overScrollBy(int deltaX, int deltaY, int scrollX,
       int scrollY, int scrollRangeX, int scrollRangeY,
       int maxOverScrollX, int maxOverScrollY, boolean isTouchEvent) 
//4.貌似最关键的地方就是在 overScrollBy 中写了这段来改变 mImageView 的高度，那么问题来了，使用mImageView.invalidate()和mImageView.requestLayout()有不一样的地方吗？
//哦！懂了，requestLayout() 是告诉parent View来重新测量设置自己的位置。invalidate() 是重新绘制自己
if (mImageView.getHeight() <= mDrawableMaxHeight && isTouchEvent) {
    if (deltaY < 0) {
        if (mImageView.getHeight() - deltaY / 2 >= mImageViewHeight) {
            mImageView.getLayoutParams().height = mImageView.getHeight() - deltaY / 2 < mDrawableMaxHeight ?
                    mImageView.getHeight() - deltaY / 2 : mDrawableMaxHeight;
            mImageView.requestLayout();
        }
    } else {
        if (mImageView.getHeight() > mImageViewHeight) {
            mImageView.getLayoutParams().height = mImageView.getHeight() - deltaY > mImageViewHeight ?
                    mImageView.getHeight() - deltaY : mImageViewHeight;
            mImageView.requestLayout();
            return true;
        }
    }
}
```

## 4.[RoundedImageView](https://github.com/vinc3m1/RoundedImageView)
**核心原理：**
```java
//1.首先是 RoundedDrawable，其主要用到了 BitmapShader 这个东东
mBitmapPaint.setShader(BitmapShader);
//2.而BitmapShader在一定条件下又会应用这种效果：
if (mTileModeX == Shader.TileMode.CLAMP && mTileModeY == Shader.TileMode.CLAMP) {
	mBitmapShader.setLocalMatrix(mShaderMatrix);
}
//3.而mShaderMatrix则是根据ScaleType直接算的，此处高潮了！最终的结果就是为了改变 BorderRect 这么一个矩形
switch (mScaleType) {
  case CENTER:
    mBorderRect.set(mBounds);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.reset();
    mShaderMatrix.setTranslate((int) ((mBorderRect.width() - mBitmapWidth) * 0.5f + 0.5f),
        (int) ((mBorderRect.height() - mBitmapHeight) * 0.5f + 0.5f));
    break;
  case CENTER_CROP:
    mBorderRect.set(mBounds);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.reset();
    dx = 0;
    dy = 0;
    if (mBitmapWidth * mBorderRect.height() > mBorderRect.width() * mBitmapHeight) {
      scale = mBorderRect.height() / (float) mBitmapHeight;
      dx = (mBorderRect.width() - mBitmapWidth * scale) * 0.5f;
    } else {
      scale = mBorderRect.width() / (float) mBitmapWidth;
      dy = (mBorderRect.height() - mBitmapHeight * scale) * 0.5f;
    }
    mShaderMatrix.setScale(scale, scale);
    mShaderMatrix.postTranslate((int) (dx + 0.5f) + mBorderWidth,
        (int) (dy + 0.5f) + mBorderWidth);
    break;
  case CENTER_INSIDE:
    mShaderMatrix.reset();
    if (mBitmapWidth <= mBounds.width() && mBitmapHeight <= mBounds.height()) {
      scale = 1.0f;
    } else {
      scale = Math.min(mBounds.width() / (float) mBitmapWidth,
          mBounds.height() / (float) mBitmapHeight);
    }
    dx = (int) ((mBounds.width() - mBitmapWidth * scale) * 0.5f + 0.5f);
    dy = (int) ((mBounds.height() - mBitmapHeight * scale) * 0.5f + 0.5f);
    mShaderMatrix.setScale(scale, scale);
    mShaderMatrix.postTranslate(dx, dy);
    mBorderRect.set(mBitmapRect);
    mShaderMatrix.mapRect(mBorderRect);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.setRectToRect(mBitmapRect, mBorderRect, Matrix.ScaleToFit.FILL);
    break;
  default:
  case FIT_CENTER:
    mBorderRect.set(mBitmapRect);
    mShaderMatrix.setRectToRect(mBitmapRect, mBounds, Matrix.ScaleToFit.CENTER);
    mShaderMatrix.mapRect(mBorderRect);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.setRectToRect(mBitmapRect, mBorderRect, Matrix.ScaleToFit.FILL);
    break;
  case FIT_END:
    mBorderRect.set(mBitmapRect);
    mShaderMatrix.setRectToRect(mBitmapRect, mBounds, Matrix.ScaleToFit.END);
    mShaderMatrix.mapRect(mBorderRect);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.setRectToRect(mBitmapRect, mBorderRect, Matrix.ScaleToFit.FILL);
    break;
  case FIT_START:
    mBorderRect.set(mBitmapRect);
    mShaderMatrix.setRectToRect(mBitmapRect, mBounds, Matrix.ScaleToFit.START);
    mShaderMatrix.mapRect(mBorderRect);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.setRectToRect(mBitmapRect, mBorderRect, Matrix.ScaleToFit.FILL);
    break;
  case FIT_XY:
    mBorderRect.set(mBounds);
    mBorderRect.inset((mBorderWidth) / 2, (mBorderWidth) / 2);
    mShaderMatrix.reset();
    mShaderMatrix.setRectToRect(mBitmapRect, mBorderRect, Matrix.ScaleToFit.FILL);
    break;
}
mDrawableRect.set(mBorderRect);
```

## 5.[dialogplus](https://github.com/orhanobut/dialogplus)
**核心原理：**
```java
//1.弹出对话框就是这两句话，作者直接将其 rootView 添加在了DecorView 上，挺聪明！
ViewGroup decorView = 
	(ViewGroup) activity.getWindow().getDecorView().findViewById(android.R.id.content);
decorView.addView(view);
//2. 其它的就是一些布局，然后配置之类的，通过 LinearLayout的weight属性可以实现 在顶部显示还是在底部显示的效果，再附有这两个颜色…就能以假乱真了！
<color name="black_overlay">#60000000</color>
<color name="white_overlay">#40FFFFFF</color>
```