Problems:
1. Firebase Authentication not cover whole website pages: copy paste validation,
   ```
   <script>
    const firebaseConfig = {
      ...
    };

    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();

    // Protect this page
    auth.onAuthStateChanged(user => {
      if (!user || !user.emailVerified) {
        window.location.href = "index.html";
      }
    });
   </script>
  
   ```
   fire base auth comein too late, after page already render <br>
   Add an “auth blocker” at the very top of <body>
   ```
   <body>
     <div id="auth-blocker" style="
       position: fixed;
       inset: 0;
       background: white;
       z-index: 99999;
       display: flex;
       justify-content: center;
       align-items: center;
       font-size: 20px;
     ">
       Checking authentication…
     </div>
   ```
   
2. User select previous date both select and input: Need submit listener handle date selection validation for manually innput
   ```
   <script>
    // ban user set previous date
    const today = new Date().toISOString().split("T")[0];
    document.getElementById("flightDate").setAttribute("min", today);

   </script>

   <script>
     // ban user manually input previous date
     document.getElementById("flightForm").addEventListener("submit", function (e) {
       const inputDate = document.getElementById("flightDate").value;
       const today = new Date().toISOString().split("T")[0];
       if (inputDate < today) {
         e.preventDefault();
         alert("不能选择过去的日期！");
         return;
       }
     });
   </script>
   
   ```
   not trigger JS alert
   ```
   看到的提示 “value must be greater than or equal to …” 不是来自 JavaScript，而是来自 HTML 内置验证（因为 会自动触发浏览器自己的验证）。
   当 HTML5 自带验证失败时，会在你的 JS 之前阻止提交，因此你的 alert 根本不会运行。
   ```
4. checkbox default not checked -> result always shows all nomatter the selection: default all change to all "checked" for checkbox option
   ```
   <!-- Class Dropdown -->
   <div class="dropdown-checkbox">
     <label for="class-select">舱等:</label>
     <button type="button" id="class-button" onclick="toggleDropdown('class')">请选择舱等 (默认全选)</button>
     <div class="options" id="class-options">
       <!-- 默认全选 -->
       <!-- <label><input type="checkbox" value="all" onchange="updateButtonText('class')"> 默认全部</label> -->
       <label><input type="checkbox" name="class" value="economy" onchange="updateButtonText('class')" checked> 经济</label>
       <label><input type="checkbox" name="class" value="super_economy" onchange="updateButtonText('class')" checked> 超级经济</label>
       <label><input type="checkbox" name="class" value="business" onchange="updateButtonText('class')" checked> 商务</label>
       <label><input type="checkbox" name="class" value="first" onchange="updateButtonText('class')" checked> 头等</label>
     </div>
   </div>

   // ----------------------
   // Update button text when checkboxes change, user has to select at least on checkbox
   // ----------------------
   function updateButtonText(type) {
     let checkboxes, button, emptyMessage;

     if (type === "class") {
        checkboxes = document.querySelectorAll('#class-options input[type="checkbox"]');
        button = document.getElementById("class-button");
        emptyMessage = "请至少选择一个舱等";
     } else if (type === "airline") {
        checkboxes = document.querySelectorAll('#airline-options input[type="checkbox"]');
        button = document.getElementById("airline-button");
        emptyMessage = "请至少选择一个航空公司";
     }

     const selected = [];
     checkboxes.forEach(cb => {
       if (cb.checked) {
         selected.push(cb.parentNode.textContent.trim());
       }
     });

     if (selected.length > 0) {
       button.textContent = selected.join(', ');
     } else {
       button.textContent = emptyMessage;
     }
   }
   ```
   
5. result page not show correct language:

6. param.get() only get the first element not whole array:
   ```
   // Prepare displayed data
   const selectionData = {
      "日期": getValue("flightDate"),
      "出发地": getValue("departure"),
      "到达地": getValue("arrival"),
      "舱等": getValue("class", "全部"), // getValue only fetch one element
      "航司": getValue("airline", "全部"),
      "仅限直飞": params.get("directFlight") ? "是" : "否"
   };
   ```
   fix to ->
   ```
   const selectionData = {
      "日期": getValue("flightDate"),
      "出发地": getValue("departure"),
      "到达地": getValue("arrival"),
      
      // multiple values support
      "舱等": (() => {
        const classes = params.getAll("class");
        return classes.length > 0 ? classes.join(", ") : "全部";
      })(),

      "航司": (() => {
        const airlines = params.getAll("airline");
        return airlines.length > 0 ? airlines.join(", ") : "全部";
      })(),
      "仅限直飞": params.get("directFlight") ? "是" : "否"
   };
   ```
