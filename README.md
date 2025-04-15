# Jogo
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Duolingo Style Game</title>
  <style>
    body { font-family: Arial; text-align: center; padding: 20px; }
    h1 { color: #4CAF50; }
    .word-pair { display: flex; justify-content: center; flex-wrap: wrap; margin-top: 20px; }
    .word { 
      padding: 15px 25px; 
      margin: 10px; 
      background-color: #e0f7fa; 
      border-radius: 8px; 
      cursor: pointer; 
      font-size: 18px;
      border: 2px solid transparent;
    }
    .selected { border: 2px solid #4CAF50; background-color: #c8e6c9; }
    #result { margin-top: 20px; font-weight: bold; }
  </style>
</head>
<body>

<h1>Combine as Palavras</h1>
<p>Toque nas palavras em inglês e português para combinar</p>

<div class="word-pair" id="words">
</div>

<div id="result"></div>

<script>
  const pairs = [
    { en: "Dog", pt: "Cachorro" },
    { en: "Book", pt: "Livro" },
    { en: "House", pt: "Casa" }
  ];

  let selected = [];
  let matches = [];

  function shuffle(array) {
    return array.sort(() => Math.random() - 0.5);
  }

  function renderWords() {
    const wordDiv = document.getElementById("words");
    wordDiv.innerHTML = '';
    const shuffled = shuffle([...pairs.map(p => ({ word: p.en, lang: 'en' })), ...pairs.map(p => ({ word: p.pt, lang: 'pt' }))]);

    shuffled.forEach((item, index) => {
      const btn = document.createElement('div');
      btn.classList.add('word');
      btn.textContent = item.word;
      btn.dataset.lang = item.lang;
      btn.dataset.index = index;
      btn.onclick = () => selectWord(btn, item.word, item.lang);
      wordDiv.appendChild(btn);
    });
  }

  function selectWord(element, word, lang) {
    if (element.classList.contains('selected')) return;

    element.classList.add('selected');
    selected.push({ word, lang, element });

    if (selected.length === 2) {
      const [w1, w2] = selected;
      const match = pairs.find(p => 
        (p.en === w1.word && p.pt === w2.word) || 
        (p.pt === w1.word && p.en === w2.word)
      );

      if (match) {
        document.getElementById("result").textContent = "Correto!";
        matches.push(match);
      } else {
        document.getElementById("result").textContent = "Errado!";
        setTimeout(() => {
          w1.element.classList.remove('selected');
          w2.element.classList.remove('selected');
        }, 800);
      }
      selected = [];

      if (matches.length === pairs.length) {
        document.getElementById("result").textContent = "Você completou todas as combinações!";
      }
    }
  }

  renderWords();
</script>

</body>
</html>
