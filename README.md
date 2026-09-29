<h2>Librus 💸</h2>

<div>
    <label for="nick">Nazwa:</label>
    <input type="text" id="nick" placeholder="Wpisz nazwę">
</div>

<script>
setInterval(() => {
    const input = document.getElementById("nick");

    input.value = input.value
        .replaceAll("1", "1️⃣")
        .replaceAll("2", "2️⃣")
        .replaceAll("3", "3️⃣")
        .replaceAll("4", "4️⃣")
        .replaceAll("5", "5️⃣");

}, 1000);
</script>
