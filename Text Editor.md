# 文本编辑器实现探索

## 文本框

`android.widget.EditText`将输入进去的字符作为String返回，只有光标在文本框里时才能输入  
使用`EditText.getSelectionStart()`方法获取光标位置  
使用`EditText.setSelection(int start, int stop)`来设置光标位置  
使用`Editable e = editText.getText()`获取文本框内的整个String  
使用`Editable.delete(int st, int en)`来进行诸如`5dd`这样的操作  
使用`Editable.insert(int where, CharSequence text)`来进行诸如`p`这样的操作  

但使用这些方法会将打开的整个代码文件全存在一个对象里。这样或许在文件较大时会使得性能下降。不过目前来说咱们应该没有打开较大文件的需求，未来如果有性能需求可以再看一些开源代码来进行探索。目前位置可以使用EditText的一系列工具实现文本框。

---

## 输入监听

```java
EditText.addTextChangedListener(new TextWatcher(){
    @Override
    public void beforeTextChanged(CharSequence charSequence, int start, int count, int after) {}

    @Override
    public void onTextChanged(CharSequence charSequence, int start, int before, int count) {}

    @Override
    public void afterTextChanged(Editable editable) {}
});
```

`android.text.TextWatcher`的`beforeTextChanged(...)`方法可以用来在输入上屏前监听输入的内容，这样可以做到直接检测vim指令并作出相应操作。

---

## 语法高亮 [highlightjs-android](https://github.com/PDDStudio/highlightjs-android)

基于`highlight.js`，也是传入String或File对象，但似乎没有使用`android.widget.EditText`

---

## 实现思路

在进一步探索之前，需要**统一语音输入的接口形式**。是以语音输入法的形式传回一个转化后的String，或者是其他的形式。这涉及到语音输入法的一些可实现性，在这里我没有进行探索。

**如果**作为语音输入法实现的话，可能难以实现**语音指令无缝**切换模式的功能。  
目前想到的是提供另一个Buffer来储存输入的vim指令，并选择性地显示在屏幕上。  
进入该模式(即Normal或Command模式)要通过触摸屏幕来进行切换，即不把`Esc`键做成输入的一个String指令，可以通过在屏幕上添加相应Esc按纽来进行切换。  

当光标切换到命令模式的`EditText`时，记录下Code的`EditText`的String和光标位置。并根据命令模式下使用`TextWatcher`监听到的指令来对该字符串进行相应操作。  
