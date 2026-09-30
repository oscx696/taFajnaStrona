<p2> "Policz groszowki" </p2>

<div>
    <input type="text" id="liczby" placeholder="Wpisz liczby">
    <button id="dodaj">➜</button>
</div>

<p id="wynik"></p>

<script>
document.getElementById("dodaj").addEventListener("click", function() {
    const tekst = document.getElementById("liczby").value;

    let suma = 0;

    for (const znak of tekst) {
        if (znak === "1") suma += 0.1;
        if (znak === "2") suma += 0.2;
        if (znak === "3") suma += 0.3;
        if (znak === "4") suma += 0.4;
        if (znak === "5") suma += 0.5;
    }

    document.getElementById("wynik").textContent = suma;
});
</script>
