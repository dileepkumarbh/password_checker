<!DOCTYPE html>
<html lang="en">

<head>
   <meta charset="UTF-8">
   <meta http-equiv="X-UA-Compatible" content="IE=edge">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Password Checker</title>
   <style>
       @import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");


body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #ffffff;
}



main {
  color: #fff;
  background-color: #b5d2f8;
  border: 2px solid #fff;
  border-radius: 10px;
  padding: 3rem;
}

#password-input {
  width: 300px;
  height: 50px;
  text-align: center;
  margin: 20px 0;
  padding: 0 10px;
  outline: none;
}

#password-constraint-container {
  display: grid;
  place-items: center;
}

.constraints {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px 0;
}

.correct,
.wrong {
  margin-left: 5px;
}

.wrong {
  display: none;
}

#password-strength-container {
  margin-top: 10px;
}

#password-strength-meter {
  width: 200px;
}

#password-strength-container div {
  display: flex;
  justify-content: center;
  align-items: center;
}

#password-strength-container p {
  margin-top: 10px;
}

   </style>

   <link rel="stylesheet" href="password_checker.css">
   <script src="password_checker.js" defer></script>

   <script src="https://kit.fontawesome.com/23538a4f23.js" crossorigin="anonymous">
       
const passwordField = document.getElementById("password-input");
passwordField.addEventListener("input", passwordCheck);

function passwordCheck() {
   const password = passwordField.value;
   const len = password.length;

   const rangeStyle = document.getElementById("range-constraint").style;
   const uppercaseStyle = document.getElementById("uppercase-constraint").style;
   const lowercaseStyle = document.getElementById("lowercase-constraint").style;
   const digitStyle = document.getElementById("digit-constraint").style;
   const splCharStyle = document.getElementById("spl-char-constraint").style;
   const meter = document.getElementById("password-strength-meter");
   const ps = document.getElementById("password-strength");

   const correct = document.querySelectorAll(".correct");
   const wrong = document.querySelectorAll(".wrong");

   const constraints = document.querySelectorAll(".constraints-para");
   const numOfConstraints = constraints.length;

   const inRange = len >= 8 && len <= 16;
   let i;
   if (len === 0) {
      rangeStyle.color = uppercaseStyle.color = lowercaseStyle.color = digitStyle.color = splCharStyle.color = "#fff";

      for (i = 0; i < numOfConstraints; i++) {
         correct[i].style.color = "#fff";
         correct[i].style.display = "inherit";
         wrong[i].style.display = "none";
      }

      meter.value = "0";
      ps.innerText = "Very Weak";
   } else {
      // checking constraints
      const uppercase = /[A-Z]/.test(password);
      const lowercase = /[a-z]/.test(password);
      const digit = /[0-9]/.test(password);
      const splChar = /[!#@?+_=\.\*]/.test(password);

      const testArray = [inRange, uppercase, lowercase, digit, splChar];

      for (i = 0; i < numOfConstraints; i++) {
         if (testArray[i]) {
            correct[i].style.display = "inherit";
            wrong[i].style.display = "none";
            constraints[i].style.color = correct[i].style.color = "#0f0";
         } else {
            correct[i].style.display = "none";
            wrong[i].style.display = "inherit";
            constraints[i].style.color = wrong[i].style.color = "#f00";
         }
      }

      if (testArray.every(v => v === true)) {
         meter.value = "100";
         ps.innerText = "Very Strong";
      } else if (uppercase && lowercase && !digit && splChar) {
         meter.value = "75";
         ps.innerText = "Strong";
      } else if (uppercase && lowercase && digit && !splChar) {
         meter.value = "50";
         ps.innerText = "Medium";
      } else if (uppercase && lowercase && !digit && !splChar) {
         meter.value = "25";
         ps.innerText = "Weak";
      } else {
         meter.value = "0";
         ps.innerText = "Very Weak";
      }
   }
}

   </script>
</head>

<body>
   <main class="display-flex-column">
      <div class="display-flex-column">
         <h2>Password Checker</h2>
         <p>Check if your password follows all the password rules</p>
      </div>

      <div id="password-container" class="display-flex-column">
         <input type="text" id="password-input" placeholder="Type your Password here">
      </div>

      <div id="password-constraint-container">
         <div id="range-container" class="constraints">
            <p id="range-constraint" class="constraints-para">Length is in range of 8 to 16</p>
            <i class="far fa-check-circle correct"></i>
            <i class="far fa-times-circle wrong"></i>
         </div>
         <div class="constraints">
            <p id="uppercase-constraint" class="constraints-para">Contains Uppercase Letter</p>
            <i class="far fa-check-circle correct"></i>
            <i class="far fa-times-circle wrong"></i>
         </div>
         <div class="constraints">
            <p id="lowercase-constraint" class="constraints-para">Contains Lowercase Letter</p>
            <i class="far fa-check-circle correct"></i>
            <i class="far fa-times-circle wrong"></i>
         </div>
         <div class="constraints">
            <p id="digit-constraint" class="constraints-para">Contains Digits</p>
            <i class="far fa-check-circle correct"></i>
            <i class="far fa-times-circle wrong"></i>
         </div>
         <div class="constraints">
            <p id="spl-char-constraint" class="constraints-para">Contains Special Character</p>
            <i class="far fa-check-circle correct"></i>
            <i class="far fa-times-circle wrong"></i>
         </div>
      </div>

      <div id="password-strength-container" class="display-flex-column">
         <div>
            <pre>Password Strength: </pre>
            <meter id="password-strength-meter" min="0" max="100"></meter>
         </div>
         <p>Password is <strong id="password-strength">Very Weak</strong></p>
      </div>
   </main>


</body>

</html>
