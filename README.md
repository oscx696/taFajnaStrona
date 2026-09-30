<div>
    <input type="text" id="liczby" placeholder="Wpisz liczby">
    <button id="dodaj">➜</button>
    <button id="ustawienia">⚙️</button>
</div>

<p id="wynik"></p>

<div id="panelUstawien" style="display: none;">
    <h3>⚙️ Ustawienia wartości</h3>

    <label>1 = <input type="number" id="wartosc1" value="1" step="0.1"></label><br>
    <label>2 = <input type="number" id="wartosc2" value="2" step="0.1"></label><br>
    <label>3 = <input type="number" id="wartosc3" value="3" step="0.1"></label><br>
    <label>4 = <input type="number" id="wartosc4" value="4" step="0.1"></label><br>
    <label>5 = <input type="number" id="wartosc5" value="5" step="0.1"></label>
</div>

<script>
const przyciskUstawien = document.getElementById("ustawienia");
const panel = document.getElementById("panelUstawien");

przyciskUstawien.addEventListener("click", function() {
    if (panel.style.display === "none") {
        panel.style.display = "block";
    } else {
        panel.style.display = "none";
    }
});

document.getElementById("dodaj").addEventListener("click", function() {
    const tekst = document.getElementById("liczby").value;

    const wartosci = {
        "1": Number(document.getElementById("wartosc1").value),
        "2": Number(document.getElementById("wartosc2").value),
        "3": Number(document.getElementById("wartosc3").value),
        "4": Number(document.getElementById("wartosc4").value),
        "5": Number(document.getElementById("wartosc5").value)
    };

    let suma = 0;

    for (const znak of tekst) {
        if (wartosci[znak] !== undefined) {
            suma += wartosci[znak];
        }
    }

    document.getElementById("wynik").textContent = suma;
});
</script>
