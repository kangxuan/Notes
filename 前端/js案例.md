### 1. 全选按钮

```html
<body>
    <table>
        <thead>
            <tr>
                <th><input type="checkbox" name="allcheck" id="allcheck"><label for="allcheck">全选</label></th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
        </tbody>
    </table>
    <script>
        // 获取全选按钮
        const allCheck = document.querySelector('#allcheck')
        // 获取子选项按钮
        const cks = document.querySelectorAll('.soncheck')
        // 当选中全选按钮则选中所有的子选项按钮
        allCheck.addEventListener('click', function(){
            for (const key in cks) {
                cks[key].checked = this.checked
            }
        })
        // 选中子选项按钮时判断一下是否所有的子选项按钮都被选中，如果是则全选按钮也要选中，反之则不选中
        for (const key in cks) {
            cks[key].addEventListener('click', function(){
                // if (cks.length == document.querySelectorAll('.soncheck:checked').length) {
                //     allCheck.checked = true
                // } else {
                //     allCheck.checked = false
                // }
                allCheck.checked = cks.length === document.querySelectorAll('.soncheck:checked').length
            })
        }
    </script>
</body>
```
