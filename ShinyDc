// ==UserScript==
// @name         Shiny Discord Notifications
// @description  Wysyła sh alerty na dc
// @author       ZHMB
// @version      0.0.1
// @match        *://*/*
// @grant        none
// @inject-into  content
// ==/UserScript==

let licznik = 0; 
function sendToDiscord(message) {
    const webhookUrl = "https://discord.com/api/webhooks/1346805344399130666/0pjpWxu1CtDEr8ja8l1DN2ar39lMQQMuCtmqSPpnm7r9aVj-BM5KgQKIqFs1skvkfcek";
    
    fetch(
        webhookUrl, // Używamy webhooka z kodu
        {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify({content: `${message}`})
        }
    )
        .catch(error => {
            console.error("Błąd przy wysyłaniu wiadomości na Discorda:", error);
        });
}

function checkForPokemon() {
    const shinyFront = document.querySelector('img[src="images/shiny_front.png"]');
    const shinyBack = document.querySelector('img[src="images/shiny_back.png"]');

    let sidebar = document.querySelector(".col-xs-6.col-sm-4.col-lg-3.sidebar-offcanvas.nopadding.nomargin");
    let panelHeading = sidebar.querySelector(".panel-heading").childNodes[0].nodeValue.trim();

    console.log('log:', panelHeading);
    licznik++;
    console.log("licznik wypraw :", licznik);

    if (shinyFront && shinyBack) {
        handleShinyEncounter(panelHeading);
    } else {
        handleShinyCapture(panelHeading);
    }
}

function handleShinyEncounter(username) {
    const textContent = document.body.textContent;
    const pokemonName = getPokemonName();
    const dxValue = getDxValue();

    if (textContent.match(/Złap Pokemona/)) {
        console.log("Złap Pokemona");
    }
    else {
         const message = `🚨 Spotkano shiny **${pokemonName}**: ${dxValue} | ${username} | wyprawy: ${licznik}`;
         licznik = 0;
         sendToDiscord(message);
    }
}

function handleShinyCapture(username) {
    const textContent = document.body.textContent;
    const nameMatch = textContent.match(/Ten wyjątkowy (.+?) bardzo źle czuje się poza swo/);

    if (nameMatch && nameMatch[1]) {
        const name = nameMatch[1].trim();
        const message = `🟢 **${name}** złapany | ${username} |`;
        sendToDiscord(message);
    }
}

function getPokemonName() {
    const pokemonNameElement = document.querySelector('.panel-heading.text-center');
    return pokemonNameElement ? pokemonNameElement.childNodes[0].textContent.trim() : '';
}

function getDxValue() {
    const dxElement = document.querySelector('button[aria-label="Współczynnik DX"]');
    return dxElement ? dxElement.textContent.trim() : '';
}

const panelElement = document.querySelector("#glowne_okno");

const observer = new MutationObserver(() => {
    checkForPokemon();
});

if (panelElement) {
    observer.observe(panelElement, {childList: true, subtree: true});
} else {
    console.log("E: Brak panelu?");
}

console.log("START");
checkForPokemon();
