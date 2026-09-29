<h2>Librus 💸</h2>

<div>
    <label for="nick">Nazwa:</label>
    <input type="text" id="nick" placeholder="Wpisz nazwę">
</div>

<script>
document.getElementById("nick").addEventListener("keydown", function(event) {
    if (event.key === "Enter") {

        this.value = this.value
            .replaceAll("1", "1️⃣")
            .replaceAll("2", "2️⃣")
            .replaceAll("3", "3️⃣")
            .replaceAll("4", "4️⃣")
            .replaceAll("5", "5️⃣");
    }
});
</script>
