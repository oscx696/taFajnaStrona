<h2>Librus 💸</h2>

<div>
    <label for="nick">Nazwa:</label>
    <input type="text" id="nick" placeholder="Wpisz nazwę">
</div>

<script>
setInterval(() => {
    const element = document.getElementById("nick");

    if (element.textContent.includes("1")) {
        element.textContent = element.textContent.replaceAll("1", "1️⃣");
    }
  if (element.textContent.includes("2")) {
        element.textContent = element.textContent.replaceAll("2", "1️2️⃣");
    }
  if (element.textContent.includes("3")) {
        element.textContent = element.textContent.replaceAll("3", "1️3️⃣");
    }
  if (element.textContent.includes("4")) {
        element.textContent = element.textContent.replaceAll("4", "1️4️⃣");
    
    }
  if (element.textContent.includes("5")) {
        element.textContent = element.textContent.replaceAll("5", "1️5️⃣");
    }
}, 1000);
</script>

