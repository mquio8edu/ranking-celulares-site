<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ranking de Produtos</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 30px;
      background-color: #f5f5f5;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    .filtro {
      text-align: center;
      margin-bottom: 20px;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background-color: #fff;
      margin-top: 10px;
    }
    th, td {
      padding: 12px;
      border: 1px solid #ddd;
      text-align: left;
    }
    th {
      background-color: #4CAF50;
      color: white;
    }
    img {
      max-width: 100px;
    }
    a {
      color: #0066cc;
      text-decoration: none;
    }
  </style>
</head>
<body>

  <h1>Ranking de Produtos por Categoria e Preço</h1>

  <div class="filtro">
    <label for="categoria">Categoria:</label>
    <select id="categoria">
      <option value="celular">Celular</option>
      <option value="notebook">Notebook</option>
      <option value="tv">TV</option>
    </select>

    <label for="preco">Faixa de Preço:</label>
    <select id="preco">
      <option value="até 1000">Até R$1000</option>
      <option value="até 2500">Até R$2500</option>
      <option value="até 3000">Até R$3000</option>
      <option value="até 2000">Até R$2000</option>
    </select>

    <button onclick="carregarRanking()">Buscar</button>
  </div>

  <table id="tabela-ranking">
    <thead>
      <tr>
        <th>Posição</th>
        <th>Modelo</th>
        <th>Imagem</th>
        <th>Oferta</th>
        <th>Fonte</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    async function carregarRanking() {
      const categoria = document.getElementById('categoria').value;
      const preco = document.getElementById('preco').value;

      const url = `https://flask-ranking-api.replit.app/ranking?categoria=${encodeURIComponent(categoria)}&faixa_preco=${encodeURIComponent(preco)}`;

      try {
        const response = await fetch(url);
        const data = await response.json();

        const tabela = document.querySelector('#tabela-ranking tbody');
        tabela.innerHTML = '';

        if (data.length === 0) {
          tabela.innerHTML = '<tr><td colspan="5">Nenhum dado encontrado.</td></tr>';
          return;
        }

        data.forEach(item => {
          const row = `
            <tr>
              <td>${item.posicao}</td>
              <td>${item.modelo}</td>
              <td><img src="${item.imagem_url || ''}" alt="${item.modelo}" /></td>
              <td><a href="${item.oferta_url}" target="_blank">Ver oferta</a></td>
              <td><a href="${item.fonte}" target="_blank">${item.fonte}</a></td>
            </tr>
          `;
          tabela.innerHTML += row;
        });

      } catch (error) {
        console.error('Erro ao carregar dados do ranking:', error);
        alert('Erro ao carregar dados. Verifique a API.');
      }
    }

    window.onload = carregarRanking;
  </script>

</body>
</html>
---

