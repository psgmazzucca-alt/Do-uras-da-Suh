<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Doçuras da Suh - Pedido de Docinhos</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Parisienne&display=swap');
body {
    font-family: 'Inter', sans-serif;
    background-color: #fce7f3; /* Rosa bem clarinho, cor de doce */
}
.header-bg {
    /* Gradiente suave com cores de doces: Rosa, Branco e Roxo */
    background: linear-gradient(to right, #f9a8d4, #ffffff, #c084fc); 
}
.section-title {
    @apply text-xl font-bold border-b-2 pb-2 mb-4 text-pink-700 border-pink-200;
}
.input-style {
    @apply w-full p-3 border border-gray-300 rounded-lg focus:ring-pink-500 focus:border-pink-500 transition duration-150;
}
/* Estilo para item selecionado (usando o foco para realçar) */
.doces-option-wrapper {
    @apply p-3 bg-white rounded-lg shadow-md hover:bg-pink-50 transition duration-150 flex items-center justify-between;
}
.doces-info {
    flex: 1;
    padding-right: 1rem;
}

/* Estilo para item desabilitado visualmente (mantido caso queira reusar) */
.doces-option input:disabled + span,
.input-style:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    background-color: #f3f4f6;
    text-decoration: line-through;
}
.special-text {
    font-family: 'Parisienne', cursive;
    @apply text-5xl text-pink-700;
}
</style>
</head>
<body class="min-h-screen p-4 flex justify-center">

<div id="app" class="w-full max-w-xl bg-white shadow-2xl rounded-xl space-y-6 md:p-0">
    <header class="text-center relative header-bg rounded-t-xl py-6 shadow-2xl">
        <img src="https://i.postimg.cc/6ppD3Td1/Imagem-do-Whats-App-de-2025-10-27-a-s-19-16-56-4c6e5526.jpg" 
             alt="Logo Doçuras da Suh" 
             class="w-40 h-40 object-cover mx-auto rounded-full mb-3 shadow-2xl border-4 border-white z-10 relative">
        <div class="px-2">
            <h1 class="text-4xl font-extrabold text-pink-900 sm:text-5xl leading-tight">Doçuras da Suh</h1>
            <p class="text-base text-pink-800 font-medium mt-1">Seus docinhos de festa, com carinho!</p>
        </div>
    </header>

    <div class="p-6 space-y-6 md:p-8 pt-0">
        
        <section id="custom-order-section" class="space-y-6 p-4 border border-pink-100 rounded-xl bg-pink-50">
            <h2 class="section-title text-pink-800">🍬 Monte Seu Pedido de Docinhos</h2>
            
            <p class="text-lg font-extrabold text-purple-600 border p-2 bg-purple-100 rounded">
                Mínimo de 50 docinhos por sabor.
            </p>

            <div class="space-y-2">
                <label for="cor-forminha" class="block font-semibold text-gray-700">🎨 1. Cor da Forminha (Obrigatório)</label>
                <input type="text" id="cor-forminha" class="input-style" placeholder="Ex: Rosa Bebê, Azul Marinho, Vermelho" required>
            </div>

            <fieldset id="doces-tradicionais-fieldset" class="space-y-3">
                <legend class="block font-bold text-gray-800 border-t pt-4 text-xl">2. Sabores Tradicionais</legend>
                <div id="doces-tradicionais-options" class="space-y-2">
                    </div>
            </fieldset>

            <fieldset id="doces-gourmet-fieldset" class="space-y-3">
                <legend class="block font-bold text-gray-800 border-t pt-4 text-xl">3. Sabores Gourmet</legend>
                <div id="doces-gourmet-options" class="space-y-2">
                    </div>
            </fieldset>
            
            <div id="doces-mistos-fieldset" style="display: none;"></div>


            <div class="space-y-2">
                <label for="obs" class="block font-semibold text-gray-700 border-t pt-4">4. Observações (Ex: Dois centos, Sem Nutella)</label>
                <input type="text" id="obs" class="input-style" placeholder="Digite aqui...">
                <p class="text-2xl font-extrabold text-green-700 mt-4 text-right">
                    Total do Pedido: <span id="item-price-display">R$ 0,00</span>
                </p>
            </div>
            
            <button id="add-to-cart-btn" onclick="addToCart()" 
                class="w-full bg-pink-600 hover:bg-pink-700 text-white font-bold py-3 rounded-lg shadow-md transition duration-300 transform hover:scale-[1.01] active:scale-[0.98]">
                ADICIONAR PEDIDO AO CARRINHO
            </button>
        </section>
        
        <section class="space-y-4 p-4 border border-gray-200 rounded-xl bg-gray-50">
            <h2 class="section-title text-gray-700 border-gray-200">🛒 Seu Pedido (<span id="cart-count">0</span> Doces no Total)</h2>
            <ul id="cart-list" class="space-y-3">
                <li id="empty-cart-message" class="text-gray-500 italic text-center">Nenhum item no carrinho.</li>
            </ul>
            <p class="text-3xl font-extrabold text-pink-700 pt-3 border-t border-gray-300 text-right">
                SUBTOTAL: <span id="subtotal-display">R$ 0,00</span>
            </p>
        </section>

        <section id="checkout-section" class="space-y-6" style="display: none;">
            <h2 class="section-title">Dados de Encomenda, Entrega e Pagamento</h2>

            <div class="space-y-3 p-3 bg-blue-50 rounded-lg border border-blue-200">
                <label class="block font-extrabold text-blue-700">🎉 Informações da Festa (Opcional)</label>
                <input type="text" id="aniversariante-nome" class="input-style" placeholder="Nome do Aniversariante">
                <input type="number" id="aniversariante-idade" class="input-style" placeholder="Idade (Ex: 1, 5, 10)">
                <input type="text" id="festa-tema" class="input-style" placeholder="Tema da Festa (Ex: Patrulha Canina, Sereia)">
            </div>
            <div class="space-y-3 p-3 bg-red-50 rounded-lg border border-red-200">
                <label class="block font-extrabold text-red-700">🗓️ Data e Horário da Encomenda (Obrigatório)</label>
                <p class="text-sm text-red-600 font-medium font-bold">⚠️ Atenção: Nossas Entregas são de Segunda a Sábado (9h às 18h). Domingo não há entrega. Agende dentro desse período.</p>
                <input type="date" id="data-encomenda" class="input-style" required>
                <input type="time" id="horario-encomenda" class="input-style" required>
            </div>

            <div class="space-y-3">
                <label class="block font-semibold text-gray-700">Seus Dados:</label>
                <input type="text" id="nome" class="input-style" placeholder="Nome Completo *" required>
                <input type="tel" id="telefone" class="input-style" placeholder="Telefone (WhatsApp) *" required>
                <input type="text" id="endereco" class="input-style" placeholder="Endereço (Rua, Número, Bairro) *" required>
                <input type="text" id="referencia" class="input-style" placeholder="Ponto de Referência (Ex: Casa verde, ao lado da praça)">
            </div>

            <div class="space-y-2">
                <label class="block font-semibold text-gray-700">Taxa de Entrega</label>
                <div id="delivery-fee-options" class="flex flex-col space-y-2">
                    <label class="flex items-center p-2 bg-white rounded-lg shadow-sm cursor-pointer">
                        <input type="radio" name="deliveryFee" value="2.00" class="form-radio text-pink-600" checked>
                        <span class="ml-2">Entrega na Cidade (R$ 2,00)</span>
                    </label>
                    <label class="flex items-center p-2 bg-white rounded-lg shadow-sm cursor-pointer">
                        <input type="radio" name="deliveryFee" value="5.00" class="form-radio text-pink-600">
                        <span class="ml-2">Entrega até 5km (R$ 5,00)</span>
                    </label>
                </div>
            </div>
            
            <div class="space-y-2">
                <label class="block font-semibold text-gray-700">Forma de Pagamento</label>
                <select id="payment-method" class="input-style" onchange="toggleTrocoField()">
                    <option value="Pix">Pix (Informar na mensagem)</option>
                    <option value="Dinheiro">Dinheiro (Precisa de troco?)</option>
                    <option value="Cartao-Debito">Cartão de Débito</option>
                    <option value="Cartao-Credito">Cartão de Crédito</option>
                </select>
            </div>
            
            <div id="troco-input-container" class="space-y-2" style="display: none;">
                <label for="troco" class="block font-semibold text-gray-700">Precisa de Troco?</label>
                <input type="text" id="troco" class="input-style" placeholder="Troco para (Ex: R$ 50,00)">
                <p class="text-sm text-gray-500">Deixe em branco se for pagar o valor exato.</p>
            </div>

            <div class="mt-6 p-4 bg-pink-100 rounded-lg shadow-inner">
                <p class="text-lg font-bold text-pink-800">Subtotal: <span id="final-subtotal">R$ 0,00</span></p>
                <p class="text-lg font-bold text-pink-800">Taxa: <span id="final-fee">R$ 2,00</span></p>
                <p class="text-4xl font-extrabold text-pink-900 mt-2">TOTAL FINAL: <span id="final-total">R$ 0,00</span></p>
            </div>

            <button id="checkout-btn" onclick="generateWhatsAppLink()" 
                class="w-full flex items-center justify-center bg-green-500 hover:bg-green-600 text-white font-extrabold py-4 rounded-lg shadow-xl transition duration-300 transform hover:scale-[1.01] active:scale-[0.98]">
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="currentColor" class="mr-2">
                    <path d="M12.001 2c-5.522 0-9.999 4.477-9.999 9.999 0 3.864 2.193 7.224 5.434 8.937l-.547 1.884c-.053.183.018.384.183.454.16.07.362.003.415-.177l.568-1.859c.772.196 1.583.298 2.404.298 5.522 0 9.999-4.477 9.999-9.999s-4.477-9.999-9.999-9.999zm4.646 12.339l-.025.048c-.627 1.127-1.428 1.176-2.923 1.025-1.579-.164-3.56-1.077-5.118-2.635-1.558-1.558-2.471-3.539-2.635-5.118-.151-1.495-.102-2.296 1.025-2.923l.048-.025.109-.052c.245-.118.528-.108.763.027l1.559.866c.216.12.339.362.31.597l-.078.618c-.035.267-.168.513-.377.674l-.234.186c-.198.158-.178.431.042.616l2.365 2.365c.184.22.457.24.616.042l.186-.234c.161-.209.407-.342.674-.377l.618-.078c.235-.029.477.094.597.31l.866 1.559c.135.235.145.518.027.763l-.052.109z"/>
                </svg>
                ENVIAR PEDIDO VIA WHATSAPP
            </button>
        </section>
    </div>

</div>

<script>
    // --- DADOS DO CARDÁPIO (FONTE ÚNICA DE VERDADE) ---
    const MENU = {
        // ✨ DADOS DE DOCINHOS
        docinhos: [
            // DOCES TRADICIONAIS
            { nome: 'Brigadeiro', tipo: 'Tradicional', preco: 130.00, descricao: 'Brigadeiro de chocolate 50% com granulado tradicional' },
            { nome: 'Leite Ninho', tipo: 'Tradicional', preco: 130.00, descricao: 'Brigadeiro de leite em pó (ninho) enrolado no ninho' },
            { nome: 'Beijinho', tipo: 'Tradicional', preco: 130.00, descricao: 'Brigadeiro de coco enrolado no flocos de coco' },
            { nome: 'Moranguinho', tipo: 'Tradicional', preco: 140.00, descricao: 'Brigadeiro de morango (nesquik) enrolado no confeito rosa' },
            { nome: 'Casadinho', tipo: 'Tradicional', preco: 160.00, descricao: 'Brigadeiro de chocolate gourmet com brigadeiro de ninho enrolado no ninho' },
            { nome: 'Paçoquinha', tipo: 'Tradicional', preco: 140.00, descricao: 'Brigadeiro de paçoquinha enrolado no amendoim' },
            { nome: 'Chocoball', tipo: 'Tradicional', preco: 140.00, descricao: 'Brigadeiro de chocolate gourmet enrolado no confeito de chocoball' }, 
            { nome: 'Brigadeiro Colorido', tipo: 'Tradicional', preco: 140.00, descricao: 'Brigadeiro de chocolate gourmet enrolado no granulado colorido' }, 
            
            // DOCES GOURMET
            { nome: 'Nozes', tipo: 'Gourmet', preco: 230.00, descricao: 'Brigadeiro de nozes, enrolado no leite ninho e decorado com nozes' },
            { nome: 'Oreo', tipo: 'Gourmet', preco: 160.00, descricao: 'Brigadeiro de oreo enrolado no biscoito triturado' },
            { nome: 'Coco Queimado', tipo: 'Gourmet', preco: 160.00, descricao: 'Brigadeiro de coco queimado enrolado em flocos de coco queimado' },
            { nome: 'Sensação', tipo: 'Gourmet', preco: 180.00, descricao: 'Brigadeiro de morango com uma pitanga de Nutella' },
            { nome: 'Maracujá', tipo: 'Gourmet', preco: 160.00, descricao: 'Brigadeiro de maracujá enrolado no leite ninho' },
            { nome: 'Confete', tipo: 'Gourmet', preco: 160.00, descricao: 'Brigadeiro de chocolate gourmet enrolado no confete' },
            { nome: 'Olho de Sogra', tipo: 'Gourmet', preco: 200.00, descricao: 'Brigadeiro de coco com ameixa enrolado no açúcar' },
            { nome: 'Churros', tipo: 'Gourmet', preco: 170.00, descricao: 'Brigadeiro de churros enrolado no açúcar e canela finalizado com uma pitanga de doce de leite' },
            { nome: 'Ferrero Rocher', tipo: 'Gourmet', preco: 170.00, descricao: 'Brigadeiro de nutella enrolado na castanha de caju com uma pitanga de nutella' },
            { nome: 'Granulado Nobre', tipo: 'Gourmet', preco: 170.00, descricao: 'Brigadeiro de chocolate 50% com granulado nobre (puro chocolate)' },
            { nome: 'Ninho com Nutella', tipo: 'Gourmet', preco: 170.00, descricao: 'Brigadeiro de leite ninho enrolado no ninho com uma pitanga de nutella' },
        ],
        
        horarios: {
            domingo: { aberto: false },
            seg_a_sexta: { inicio: 9, fim: 18 },
            sabado: { inicio: 9, fim: 18 } // HORÁRIO DE SÁBADO ATUALIZADO
        },

        // 👇 TROQUE AQUI PELO SEU NÚMERO REAL 👇
        whatsappNumber: "5517997381858",
        pixData: {
            name: "SUÉLEM CRISTINA MAESTRE MAZZUCCA",
            key: "41756000867 (CPF)"
        }
    };

    // --- ESTADO GLOBAL ---
    let cart = []; 

    // --- REFERÊNCIAS DOM ---
    const docesTradicionaisDiv = document.getElementById('doces-tradicionais-options');
    const docesGourmetDiv = document.getElementById('doces-gourmet-options');
    const itemPriceDisplay = document.getElementById('item-price-display');
    const cartList = document.getElementById('cart-list');
    const checkoutSection = document.getElementById('checkout-section');
    const deliveryFeeRadios = document.querySelectorAll('input[name="deliveryFee"]');
    const addToCartBtn = document.getElementById('add-to-cart-btn');
    const trocoInputContainer = document.getElementById('troco-input-container');
    const paymentMethodSelect = document.getElementById('payment-method');

    // --- LÓGICA DE VALIDAÇÃO DE DATA E HORÁRIO ---

    function checkDomingo(event) {
        const dataInput = document.getElementById('data-encomenda');
        const selectedDate = new Date(dataInput.value + 'T00:00:00'); 
        if (selectedDate.getDay() === 0) { 
            alert('Não realizamos entregas aos domingos. Por favor, escolha outra data.');
            dataInput.value = '';
        }
    }
    
    function isTimeValid(dateString, timeString) {
        const selectedDate = new Date(dateString + 'T00:00:00');
        const day = selectedDate.getDay();
        const [hour, minute] = timeString.split(':').map(Number);
        
        if (day === 0) return false; // Domingo

        // De Segunda (1) a Sábado (6)
        if (day >= 1 && day <= 6) { 
            const inicio = MENU.horarios.seg_a_sexta.inicio; // 9
            const fim = MENU.horarios.seg_a_sexta.fim; // 18 (seja segunda a sexta ou sábado)

            if (hour < inicio || hour >= fim) return false;
            // Se for exatamente o horário final, mas com minutos > 0 (ex: 18:01)
            if (hour === fim && minute > 0) return false; 
        }

        return true;
    }


    // --- FUNÇÕES DE LÓGICA E RENDERIZAÇÃO PRINCIPAL ---
    
    /**
     * Renderiza as opções de docinhos com o novo campo de quantidade.
     */
    function renderDocinhos() {
        const docesPorTipo = MENU.docinhos.reduce((acc, doce) => {
            acc[doce.tipo] = acc[doce.tipo] || [];
            acc[doce.tipo].push(doce);
            return acc;
        }, {});

        const renderGroup = (container, doces) => {
            container.innerHTML = doces.map(doce => {
                const id = `qty-${doce.nome.replace(/\s/g, '-')}`;

                return `
                    <div class="doces-option-wrapper">
                        <div class="doces-info">
                            <span class="font-bold text-gray-800">${doce.nome}</span> 
                            <span class="text-sm text-gray-600 block">${doce.descricao}</span>
                            <span class="font-bold text-red-600 block">R$ ${doce.preco.toFixed(2).replace('.', ',')} (Cento)</span>
                        </div>
                        <div class="flex items-center space-x-2">
                            <label for="${id}" class="text-sm font-semibold text-gray-700">Qtd (50+):</label>
                            <input type="number" id="${id}" name="doce-quantidade" 
                                data-nome="${doce.nome}"
                                data-preco-cento="${doce.preco}"
                                value="0" min="0" step="50" 
                                class="w-20 p-2 text-center border border-gray-300 rounded-lg focus:ring-pink-500 focus:border-pink-500"
                                onchange="updateItemPrice(this)"
                                onkeyup="updateItemPrice(this)">
                        </div>
                    </div>
                `;
            }).join('');
        };

        renderGroup(docesTradicionaisDiv, docesPorTipo['Tradicional'] || []);
        renderGroup(docesGourmetDiv, docesPorTipo['Gourmet'] || []);
        
        // Inicializa o preço total
        updateItemPrice(); 
        updateAddToCartButtonState();
    }

    /**
     * Função auxiliar para alternar a exibição do campo de troco.
     */
    function toggleTrocoField() {
        const paymentMethod = paymentMethodSelect.value;
        trocoInputContainer.style.display = paymentMethod === 'Dinheiro' ? 'block' : 'none';
        
    }

    /**
     * Calcula o preço total do item atual (soma de todos os sabores com suas quantidades).
     */
    function updateItemPrice() {
        let precoTotalPedido = 0;
        const inputs = document.querySelectorAll('input[name="doce-quantidade"]');

        inputs.forEach(input => {
            const quantidade = parseInt(input.value) || 0;
            const precoCento = parseFloat(input.dataset.precoCento);
            
            // Validação visual (multiplo de 50)
            if (quantidade > 0 && quantidade % 50 !== 0) {
                input.classList.add('border-red-500'); 
            } else {
                input.classList.remove('border-red-500'); 
            }
            
            // Preço por doce = Preço do cento / 100
            const precoUnitario = precoCento / 100;
            
            precoTotalPedido += precoUnitario * quantidade;
        });

        itemPriceDisplay.textContent = `R$ ${precoTotalPedido.toFixed(2).replace('.', ',')}`;
        updateAddToCartButtonState();
    }
    
    /**
     * Retorna os dados do pedido atual (todos os sabores e quantidades).
     */
    function getFormData() {
        const corForminha = document.getElementById('cor-forminha').value.trim();
        const inputs = document.querySelectorAll('input[name="doce-quantidade"]');
        let saboresDetalhes = [];
        let precoTotal = 0;
        let totalDoces = 0;
        let isValid = true; // Flag para rastrear a validação
        
        if (!corForminha) {
            alert('Por favor, preencha a cor da forminha.');
            return null;
        }

        inputs.forEach(input => {
            const quantidade = parseInt(input.value) || 0;
            
            if (quantidade > 0) {
                // 1. Validação do mínimo e do passo (50 em 50)
                if (quantidade < 50 || quantidade % 50 !== 0) {
                    alert(`O sabor "${input.dataset.nome}" deve ter uma quantidade mínima de 50 e ser múltiplo de 50. Por favor, ajuste.`);
                    input.focus();
                    isValid = false; // Define a flag como false
                    return; // Sai deste loop forEach
                }

                const precoCento = parseFloat(input.dataset.precoCento);
                const precoUnitario = precoCento / 100;
                const precoSubtotal = precoUnitario * quantidade;

                precoTotal += precoSubtotal;
                totalDoces += quantidade;

                saboresDetalhes.push({
                    nome: input.dataset.nome,
                    quantidade: quantidade,
                    precoSubtotal: precoSubtotal
                });
            }
        });
        
        if (!isValid) return null; // Retorna nulo se a validação falhou
        
        if (saboresDetalhes.length === 0 || totalDoces === 0) {
            alert('Por favor, selecione a quantidade para pelo menos um sabor.');
            return null;
        }
        
        const customItem = {
            name: `Pedido Customizado (${totalDoces} Doces)`,
            price: precoTotal, 
            totalDoces: totalDoces,
            corForminha: corForminha,
            sabores: saboresDetalhes,
            obs: document.getElementById('obs').value.trim()
        };

        return customItem;
    }
    
    /**
     * Habilita/Desabilita o botão Adicionar ao Carrinho.
     */
    function updateAddToCartButtonState() {
        const totalDoces = Array.from(document.querySelectorAll('input[name="doce-quantidade"]')).reduce((sum, input) => sum + (parseInt(input.value) || 0), 0);
        const corForminha = document.getElementById('cor-forminha').value.trim();
        
        const isDisabled = totalDoces === 0 || !corForminha;

        addToCartBtn.disabled = isDisabled;
        addToCartBtn.textContent = isDisabled 
                                    ? 'PREENCHA A COR DA FORMINHA E AS QUANTIDADES' 
                                    : 'ADICIONAR PEDIDO AO CARRINHO';
        if(isDisabled) {
            addToCartBtn.classList.remove('bg-pink-600', 'hover:bg-pink-700');
            addToCartBtn.classList.add('bg-gray-400', 'cursor-not-allowed');
        } else {
            addToCartBtn.classList.remove('bg-gray-400', 'cursor-not-allowed');
            addToCartBtn.classList.add('bg-pink-600', 'hover:bg-pink-700');
        }
    }

    function addToCart() {
        const item = getFormData();
        if (!item) return;

        cart.push(item);
        
        // Reseta o estado para o próximo item
        document.getElementById('cor-forminha').value = '';
        document.getElementById('obs').value = '';
        document.querySelectorAll('input[name="doce-quantidade"]').forEach(input => input.value = '0');
        
        renderDocinhos(); 
        renderCart();
    }

    function renderCart() {
        let subtotal = 0;
        let cartItemsHtml = '';
        let totalDocesCarrinho = 0;

        cart.forEach((item, index) => {
            subtotal += item.price;
            totalDocesCarrinho += item.totalDoces; 
            
            const detalhesSabores = item.sabores.map(s => `(${s.quantidade}x) ${s.nome}`).join(' | ');

            cartItemsHtml += `
                <li class="p-3 bg-pink-50 border-l-4 border-pink-500 rounded-lg shadow-sm flex justify-between items-center">
                    <div class="flex-1">
                        <p class="font-bold text-pink-800">${item.name}</p>
                        <p class="text-sm text-gray-600 truncate">Cor: ${item.corForminha} | Sabores: ${detalhesSabores}</p>
                        ${item.obs ? `<p class="text-xs text-red-500 italic">Obs: ${item.obs}</p>` : ''}
                    </div>
                    <div class="flex items-center space-x-2">
                        <p class="font-extrabold text-lg text-pink-700">R$ ${item.price.toFixed(2).replace('.', ',')}</p>
                        <button onclick="removeFromCart(${index})" class="text-red-500 hover:text-red-700 text-xl font-bold p-1 leading-none rounded-full transition duration-150" aria-label="Remover item">
                            &times;
                        </button>
                    </div>
                </li>
            `;
        });

        document.getElementById('subtotal-display').textContent = `R$ ${subtotal.toFixed(2).replace('.', ',')}`;
        document.getElementById('cart-count').textContent = totalDocesCarrinho; 

        cartList.innerHTML = cart.length > 0 ? cartItemsHtml : '<li id="empty-cart-message" class="text-gray-500 italic text-center">Nenhum item no carrinho.</li>';

        checkoutSection.style.display = cart.length > 0 ? 'block' : 'none';

        updateFinalSummary(subtotal);
    }
    
    function removeFromCart(index) {
        cart.splice(index, 1);
        renderCart();
    }

    function updateFinalSummary(subtotal) {
        let currentSubtotal = subtotal;
        if (typeof currentSubtotal !== 'number') {
            currentSubtotal = cart.reduce((total, item) => total + item.price, 0);
        }
        
        // Garante que o valor da taxa seja atualizado se o usuário mudar o radio button
        const deliveryFee = parseFloat(document.querySelector('input[name="deliveryFee"]:checked')?.value || 0);
        const finalTotal = currentSubtotal + deliveryFee;
        
        document.getElementById('final-subtotal').textContent = `R$ ${currentSubtotal.toFixed(2).replace('.', ',')}`;
        document.getElementById('final-fee').textContent = `R$ ${deliveryFee.toFixed(2).replace('.', ',')}`;
        document.getElementById('final-total').textContent = `R$ ${finalTotal.toFixed(2).replace('.', ',')}`;
    }
    
    function generateWhatsAppLink() {
        // Campos Obrigatórios
        const nome = document.getElementById('nome').value.trim();
        const telefone = document.getElementById('telefone').value.trim();
        const endereco = document.getElementById('endereco').value.trim();
        const dataEncomenda = document.getElementById('data-encomenda').value;
        const horarioEncomenda = document.getElementById('horario-encomenda').value;
        
        // Novos Campos (Opcionais)
        const aniversarianteNome = document.getElementById('aniversariante-nome').value.trim();
        const aniversarianteIdade = document.getElementById('aniversariante-idade').value.trim();
        const festaTema = document.getElementById('festa-tema').value.trim();


        if (!nome || !telefone || !endereco || !dataEncomenda || !horarioEncomenda) {
            alert('Por favor, preencha todos os campos obrigatórios (Nome, Telefone, Endereço, Data e Horário da Encomenda).');
            return;
        }
        
        // Validação de horário atualizada
        if (!isTimeValid(dataEncomenda, horarioEncomenda)) {
             alert('O horário de entrega selecionado está fora do nosso horário de funcionamento (Seg a Sáb: 9h às 18h). Por favor, ajuste o horário ou a data.');
             return;
        }

        let message = `*Olá! Sou ${nome} e gostaria de fazer o seguinte pedido de Docinhos:*\n\n`;
        let subtotal = 0;
        const deliveryFee = parseFloat(document.querySelector('input[name="deliveryFee"]:checked').value);
        const feeDescription = document.querySelector('input[name="deliveryFee"]:checked').nextElementSibling.textContent;

        // Detalhes dos Docinhos
        cart.forEach((item, index) => {
            subtotal += item.price;
            message += `*--- Pedido ${index + 1} (${item.totalDoces} Doces) ---*\n`;
            message += `*Cor da Forminha:* ${item.corForminha}\n`;
            
            item.sabores.forEach(sabor => {
                 message += `- ${sabor.quantidade}x ${sabor.nome}\n`;
            });
            
            message += `*Subtotal dos Doces:* R$ ${item.price.toFixed(2).replace('.', ',')}\n`;
            if (item.obs) message += `*Obs do Pedido:* ${item.obs}\n`;
            message += `\n`;
        });

        const finalTotal = subtotal + deliveryFee;

        // Dados do Cliente e Festa
        message += `*--- DADOS DA ENCOMENDA/CLIENTE ---*\n`;
        message += `*Nome:* ${nome}\n`;
        message += `*Telefone:* ${telefone}\n`;
        
        // Dados da Festa (opcionais)
        if (aniversarianteNome || aniversarianteIdade || festaTema) {
            message += `\n*--- INFORMAÇÕES DA FESTA ---*\n`;
            if (aniversarianteNome) message += `*Aniversariante:* ${aniversarianteNome}\n`;
            if (aniversarianteIdade) message += `*Idade:* ${aniversarianteIdade} anos\n`;
            if (festaTema) message += `*Tema:* ${festaTema}\n`;
        }

        message += `\n*--- ENTREGA E PAGAMENTO ---*\n`;
        message += `*Data da Encomenda:* ${dataEncomenda.split('-').reverse().join('/')}\n`;
        message += `*Horário de Entrega:* ${horarioEncomenda}\n`;
        message += `*Endereço:* ${endereco}\n`;
        const referencia = document.getElementById('referencia').value.trim();
        if (referencia) message += `*Referência:* ${referencia}\n`;
        
        // Pagamento e Totais
        message += `\n*--- VALORES FINAIS ---*\n`;
        message += `*Subtotal Docinhos:* R$ ${subtotal.toFixed(2).replace('.', ',')}\n`;
        message += `*Taxa de Entrega:* ${feeDescription}\n`;
        message += `*TOTAL GERAL:* R$ ${finalTotal.toFixed(2).replace('.', ',')}\n`;

        const paymentMethod = document.getElementById('payment-method').value;
        message += `*Forma de Pagamento:* ${paymentMethod}\n`;

        if (paymentMethod === 'Dinheiro') {
            const troco = document.getElementById('troco').value.trim();
            message += `*Troco para:* ${troco || 'Valor Exato'}\n`;
        } else if (paymentMethod === 'Pix') {
            message += `*Dados Pix:* ${MENU.pixData.key} (${MENU.pixData.name})\n`;
        }
        
        message += `\n*Obrigado por escolher a Doçuras da Suh!*`;

        const encodedMessage = encodeURIComponent(message);
        const whatsappLink = `https://api.whatsapp.com/send?phone=${MENU.whatsappNumber}&text=${encodedMessage}`;

        // Abre o link
        window.open(whatsappLink, '_blank');
        
        // Limpa o carrinho e reseta a UI após o envio
        cart = [];
        renderCart();
    }
    
    // --- FUNÇÃO DE INICIALIZAÇÃO ---
    function setupUI() {
        renderDocinhos(); // Carrega o menu de docinhos ao iniciar
        renderCart(); // Garante que o carrinho comece vazio
        toggleTrocoField(); // Garante que o campo de troco comece oculto
        // Adiciona listeners para atualizar o sumário final ao mudar a taxa
        deliveryFeeRadios.forEach(radio => radio.addEventListener('change', () => updateFinalSummary()));
        // Adiciona listener para validação de domingo
        document.getElementById('data-encomenda').addEventListener('change', checkDomingo);
    }

    // Inicializa a UI
    setupUI();
</script>
</body>
</html>
