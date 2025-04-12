#<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Password Game Clone</title>
  <style>
    body { font-family: sans-serif; padding: 2em; background: #fdf6e3; }
    input { font-size: 1.2em; padding: 0.5em; width: 100%; }
    .rule { margin: 1em 0; padding: 0.5em; border-radius: 5px; }
    .valid { background: #d4edda; color: #155724; }
    .invalid { background: #f8d7da; color: #721c24; }
  </style>
</head>
<body>
  <h1>The Password Game</h1>
  <input type="text" id="password" placeholder="Enter your password" />
  <div id="rules"></div>  <script>
    const passwordInput = document.getElementById('password');
    const rulesDiv = document.getElementById('rules');

    function validatePassword(pwd) {
      const rules = [];

      // Rule 1: At least 5 characters
      rules.push({
        text: 'Пароль должен содержать не менее 5 символов.',
        valid: pwd.length >= 5
      });

      // Rule 2: Must include a digit
      rules.push({
        text: 'Пароль должен включать цифру.',
        valid: /\d/.test(pwd)
      });

      // Rule 3: Must include uppercase
      rules.push({
        text: 'Пароль должен содержать заглавную букву.',
        valid: /[A-Z]/.test(pwd)
      });

      // Rule 4: Must include special character
      rules.push({
        text: 'Пароль должен содержать специальный символ.',
        valid: /[^a-zA-Z0-9]/.test(pwd)
      });

      // Rule 5: Sum of digits must be 25
      const digits = pwd.match(/\d/g) || [];
      const digitSum = digits.reduce((sum, d) => sum + parseInt(d), 0);
      rules.push({
        text: 'Сумма цифр в пароле должна быть равна 25.',
        valid: digitSum === 25
      });

      // Rule 9: Roman numerals must be divisible by 35 (optional)
      const romanMatch = pwd.match(/\b[IVXLCDM]+\b/g);
      const romanValid = !romanMatch || romanMatch.every(r => romanToInt(r) % 35 === 0);
      rules.push({
        text: 'Если в пароле есть римское число, оно должно быть кратно 35.',
        valid: romanValid
      });

      return rules;
    }

    function romanToInt(roman) {
      const map = {I:1,V:5,X:10,L:50,C:100,D:500,M:1000};
      let total = 0;
      for (let i = 0; i < roman.length; i++) {
        const cur = map[roman[i]];
        const next = map[roman[i+1]];
        total += next > cur ? next - cur : cur;
        if (next > cur) i++;
      }
      return total;
    }

    passwordInput.addEventListener('input', () => {
      const pwd = passwordInput.value;
      const validations = validatePassword(pwd);
      rulesDiv.innerHTML = '';
      validations.forEach(rule => {
        const div = document.createElement('div');
        div.className = 'rule ' + (rule.valid ? 'valid' : 'invalid');
        div.textContent = (rule.valid ? '✔ ' : '✖ ') + rule.text;
        rulesDiv.appendChild(div);
      });
    });
  </script></body>
</html>
