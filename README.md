<input type="text" id="nick" placeholder="Wpisz nazwę">

<p id="wynik"></p>

<script>
const input = document.getElementById("nick");
const wynik = document.getElementById("wynik");

input.addEventListener("keydown", function(event) {
    if (event.key === "Enter") {
        wynik.textContent = this.value
            .replaceAll("1", "1️⃣")
            .replaceAll("2", "2️⃣")
            .replaceAll("3", "3️⃣")
            .replaceAll("4", "4️⃣")
            .replaceAll("5", "5️⃣");
    }
});
</script>
