const meses = [
    "Janeiro",
    "Fevereiro",
    "Março",
    "Abril",
    "Maio",
    "Junho",
    "Julho",
    "Agosto",
    "Setembro",
    "Outubro",
    "Novembro",
    "Dezembro",
];

const semana = [
    "Domingo",
    "Segunda-Feira",
    "Terça-Feira",
    "Quarta-Feira",
    "Quinta-Feira",
    "Sexta-Feira",
    "Sábado",
];

const giveaway = document.querySelector('.giveaway');
const deadline = document.querySelector('.deadline');
const itens = document.querySelectorAll('.time-format h4');

// Obter a data do usuário usando window.prompt
const userInput = prompt('Por favor, insira uma data no formato "DIA/MÊS/ANO hh:mm"');
const userDateArray = userInput.split(' ');

// Validar se a entrada do usuário é uma data válida
if (userDateArray.length !== 2) {
    alert('Formato de data inválido. Por favor, recarregue a página e insira uma data válida.');
} else {
    const dataPartes = userDateArray[0].split('/');
    const horaPartes = userDateArray[1].split(':');
    const dia = parseInt(dataPartes[0]);
    const mes = parseInt(dataPartes[1]) - 1; // Os meses em JavaScript começam do zero
    const ano = parseInt(dataPartes[2]);
    const horas = parseInt(horaPartes[0]);
    const minutos = parseInt(horaPartes[1]);
    // Validar limites
    if (
        dia < 1 || dia > 31 ||
        mes < 0 || mes > 11 ||
        ano < new Date().getFullYear() || // Garante que o ano seja igual ou superior ao ano atual
        horas < 0 || horas > 23 ||
        minutos < 0 || minutos > 59
    ) {
        alert('Valores fora dos limites permitidos. Por favor, recarregue a página e insira uma data válida.');
    } else {
        const userDate = new Date(ano, mes, dia, horas, minutos);
        const year = userDate.getFullYear();
        const diaSemana = semana[userDate.getDay()];
        const day = userDate.getDate();
        const month = meses[userDate.getMonth()];
        const hours = userDate.getHours();
        const minutes = userDate.getMinutes();
        giveaway.textContent = `Seu evento termina ${diaSemana}, ${day} de ${month} de ${year} às ${hours}:${minutes}h.`;
        // Tempo futuro - futureTime em milissegundos
        const futureTime = userDate.getTime();
        function getRemainingTime() {
            const hoje = new Date().getTime();
            let hj = futureTime - hoje;
            // 1 segundo = 1000ms
            // 1 minuto = 60 segundos
            // 1 hora = 60 minutos
            // 1 dia = 24 horas
            // valores em milissegundos:
            const umDia = 24 * 60 * 60 * 1000;
            const umaHora = 60 * 60 * 1000;
            const umMin = 60 * 1000;
            let dias = hj / umDia;
            dias = Math.floor(dias);
            let hora = Math.floor((hj % umDia) / umaHora);
            let minutos = Math.floor((hj % umaHora) / umMin);
            let segundos = Math.floor((hj % umMin) / 1000);
            const valor = [dias, hora, minutos, segundos];
            function formato(item) {
                if (item < 10) {
                    return item = `0 ${item}`;
                }
                return item;
            }
            itens.forEach(function (item, index) {
                item.innerHTML = formato(valor[index]);
            });
            if (hj < 0) {
                clearInterval(cr);
                deadline.innerHTML = `<h4 class="ex">Acabou o tempo</h4>`;
            }
        }
        // Contagem regressiva
        let cr = setInterval(getRemainingTime, 1000);
        getRemainingTime();
    }
}

//  Created by Anabelle de Moraes - All rights reserved 
