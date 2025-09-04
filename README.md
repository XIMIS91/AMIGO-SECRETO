const friendInput = document.getElementById('friendInput');
const addBtn = document.getElementById('addBtn');
const friendsList = document.getElementById('friendsList');
const drawBtn = document.getElementById('drawBtn');
const resultDiv = document.getElementById('result');

const MAX_FRIENDS = 5;

let friends = [];

function renderList() {
  friendsList.innerHTML = '';

  friends.forEach((name, index) => {
    const li = document.createElement('li');
    li.textContent = `${index + 1}. ${name}`;
    li.dataset.index = index;
    friendsList.appendChild(li);
  });

  addBtn.disabled = friends.length >= MAX_FRIENDS;

  drawBtn.disabled = friends.length === 0;
}

function addFriend() {
  const rawName = friendInput.value;
  const name = rawName.trim();

  if (name === '') {
    alert('Por favor, inserte un nombre.');
    friendInput.focus();
    return;
  }

  if (friends.length >= MAX_FRIENDS) {
    alert(`La lista no puede tener más de ${MAX_FRIENDS} nombres.`);
    return;
  }

  if (friends.includes(name)) {
    alert('Este nombre ya está en la lista.');
    friendInput.value = '';
    friendInput.focus();
    return;
  }

  friends.push(name);

 
  friendInput.value = '';
  renderList();
}


function drawFriend() {
  if (friends.length === 0) {
    alert('No hay amigos para sortear.');
    return;
  }

 
  const randomIndex = Math.floor(Math.random() * friends.length);
  const chosenName = friends[randomIndex];


  resultDiv.innerHTML = `<p>🎉 ¡El amigo secreto es: <strong>${chosenName}</strong> 🎉</p>`;

  friendsList.querySelectorAll('li').forEach(li => li.classList.remove('highlight'));
  const chosenLi = friendsList.querySelector(`li[data-index="${randomIndex}"]`);
  if (chosenLi) chosenLi.classList.add('highlight');
}


addBtn.addEventListener('click', addFriend);
drawBtn.addEventListener('click', drawFriend);


friendInput.addEventListener('keyup', (e) => {
  if (e.key === 'Enter') addFriend();
});


document.addEventListener('DOMContentLoaded', renderList);
