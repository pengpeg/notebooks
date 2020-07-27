# 自定义Python编程规范

## 目录

* <a href='fggf'>Python风格规范</a>
  - <a href='fggf_sj'>缩进</a>
* 

















## <a name='fggf'>Python风格规范</a>

### <a name='fggf_sj'>缩进</a>

<table>
    <tr><th>Tip</th></tr>
    <tr><th>用4个空格来缩进代码</th></tr>
</table>

绝对不要用tab, 也不要tab和空格混用.

```python
Yes:   # Aligned with opening delimiter
       foo = long_function_name(var_one, var_two,
                                var_three, var_four)

       # Aligned with opening delimiter in a dictionary
       foo = {
           long_dictionary_key: value1 +
                                value2,
           ...
       }

       # 4-space hanging indent; nothing on first line
       foo = long_function_name(
           var_one, var_two, var_three,
           var_four)

       # 4-space hanging indent in a dictionary
       foo = {
           long_dictionary_key:
               long_dictionary_value,
           ...
       }
```

```
No:    # Stuff on first line forbidden
      foo = long_function_name(var_one, var_two,
          var_three, var_four)

      # 2-space hanging indent forbidden
      foo = long_function_name(
        var_one, var_two, var_three,
        var_four)

      # No hanging indent in a dictionary
      foo = {
          long_dictionary_key:
              long_dictionary_value,
              ...
      }
```





