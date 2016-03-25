---
title: "opensource project 2"
layout: post
date: 2015-04-11
tags:
- opensource
categories:
- android
---

## 1. [CollapsingHeader](https://github.com/lynfogeek/CollapsingHeader)

[作者文章介绍](http://arnaud-camus.fr/material-design-extended-toolbar-and-scrolling/)

```java
//1.监听滑动事件
if (view != null && view.getChildCount() > 0 && firstVisibleItem == 0) { 
    if (view.getChildAt(0).getTop() < -dpToPx(16)) {
        toggleHeader(false, false);
    } else {
        toggleHeader(true, true);
    }
}

//2.ObjectAnimation控制alpha
private void toggleHeader(boolean visible, boolean force) {
    if ((force && visible) || (visible && mContainerHeader.getAlpha() == 0f)) {
        fade.setFloatValues(mContainerHeader.getAlpha(), 1f);
        fade.start();
    } else if (force || (!visible && mContainerHeader.getAlpha() == 1f)){
        fade.setFloatValues(mContainerHeader.getAlpha(), 0f);
        fade.start();
    }
    // Toggle the visibility of the title.
    if (getSupportActionBar() != null) {
        getSupportActionBar().setDisplayShowTitleEnabled(!visible);
    }
}

//3.让按钮跟着第一个HeaderView
public void onScroll(AbsListView view,
                     int firstVisibleItem,
                     int visibleItemCount,
                     int totalItemCount) {
    if (view != null && view.getChildCount() > 0 && firstVisibleItem == 0) {
        int translation = view.getChildAt(0).getHeight() + view.getChildAt(0).getTop();
        mFab.setTranslationY(translation>0  ? translation : 0);
    }

    if (isLollipop()) {
        if (firstVisibleItem == 0) {
            mToolbar.setElevation(0);
        } else {
            mToolbar.setElevation(dpToPx(4));
        }
    }
}
```

## 2. [CreditsRoll](https://github.com/frakbot/CreditsRoll)
```java
//[Note:]在onDraw中实际的内容：
int contentWidth = getWidth() - mPaddingLeft - mPaddingRight;
int contentHeight = getHeight() - mPaddingTop - mPaddingBottom;

// 1. StaticLayout 是一个新内容啊

// 2. 核心的 onDraw(Canvas) 中的内容：
final int saveCnt = canvas.save();

// Rotate/translate the camera
canvas.getMatrix(mMatrix);
mCamera.save();

int cX = contentWidth / 2 + mPaddingLeft;
int cY = contentHeight / 2 + mPaddingTop;
mCamera.rotateX(mAngle);
mCamera.translate(0, 0, mDistanceFromText);
mCamera.getMatrix(mMatrix);
mMatrix.preTranslate(-cX, -cY);
mMatrix.postTranslate(cX, cY);
mCamera.restore();

canvas.concat(mMatrix);

// The end scroll multiplier ensures that the text scrolls completely out of view
canvas.translate(0f, contentHeight - mScrollPosition *
                                     (mTextLayout.getHeight() + mEndScrollMult * contentHeight));

// Draw the text
mTextLayout.draw(canvas);

canvas.restoreToCount(saveCnt);

```

## 3. [material-menu](https://github.com/balysv/material-menu)

实现Android 5.0 Drawer menu 的那种动画切换效果

```java
//不想解释什么，智商明显不够…

```

## 4. [PersistentSearch](https://github.com/Quinny898/PersistentSearch)

实现Google Market的那种搜索框

```java
// DecorView 上的故事：
FrameLayout layout = (FrameLayout) activity.getWindow().getDecorView()
	.findViewById(android.R.id.content);

// LayoutTransition 上面的故事
RelativeLayout searchRoot = (RelativeLayout) findViewById(R.id.search_root);
LayoutTransition lt = new LayoutTransition();
lt.setDuration(100);
searchRoot.setLayoutTransition(lt);

```

## 5. [PreferenceInjector](https://github.com/denley/PreferenceInjector)
