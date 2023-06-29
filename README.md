# rpa_studies
Resumos de estudos Sobre Robotic Automatic Process

## A biblioteca que utilizei para esse estudo foi a Pupteer e estou utilizando node.js



### Resumo
Uma das dificuldades encontradas testando a biblioteca, foi perceber que as ações muitas vezes atropelam umas as outras já que ao programar em node.js utilizamos programação assincrona

#### Exemplo:

	const puppeteer = require('puppeteer');

	async function run() {
			const browser = await puppeteer.launch();
			const page = await browser.newPage();
		
		await page.goto('https://www.example.com/login');
		
		// Preencha os campos de login e senha e clique no botão de login.
		await page.type('#username', 'myusername');
		await page.type('#password', 'mypassword');
		await Promise.all([
				page.waitForNavigation(), // Espera até a navegação terminar
				page.click('#login-button'), // Clica no botão de login
		]);

		// Aguarde até que o botão esteja disponível na página após o login.
		await page.waitForSelector('#some-button');
		await page.click('#some-button');

		await browser.close();
	}
 
	run().catch(console.error);

  
  Após o codigo acima que foi feito o login em um sistema, se eu precisar clicar em uma serie de botoes e áreas que muitas vezes não foram geradas no hmtl pelo javascript,será preciso setar um tempo para cada ação e é muito provavel que ocorram variações de internet em cada computador e trave tentando clicar em algo que não esta na pagina ou ficar parado por muito tempo.Para resolver podemos utilizar as funções waitFor segue a baixo a listagem e como utiulizar:

 ### waitForNavigation()    
   A função retorna uma Promise que se resolve para a resposta do recurso principal. No caso de múltiplos redirecionamentos, a navegação se resolverá com a resposta do último redirecionamento. Se a navegação for para um âncora diferente ou devido ao uso da API de Histórico, a navegação será resolvida com null.
Os parâmetros opcionais de navegação podem ter as seguintes propriedades:
waitForOptions: Parâmetros de navegação (opcionais).
	 A função retorna uma Promise que resolve para a resposta do recurso principal:
	No caso de vários redirecionamentos, a navegação será resolvida com a resposta do último redirecionamento.
	No caso de navegação para um âncora diferente ou navegação devido ao uso da API de Histórico, a navegação será resolvida com null​1​.
	Aqui está um exemplo de como usar a função waitForNavigation():
			
    const [response] = await Promise.all([
			page.waitForNavigation(),  A promise resolve após a navegação ter terminado
			page.click('a.my-link'),Clicar no link causará indiretamente uma navegação
  	]);

Neste exemplo, a promessa page.waitForNavigation() é resolvida após a navegação ter terminado, que é acionada pelo clique no link a.my-link​1​.

Os erros comuns ao usar waitForNavigation são:

### Timeout:
Se a navegação demorar muito para ocorrer, um erro de tempo limite pode ser lançado. Você pode ajustar o tempo limite padrão através da opção timeout no objeto de opções.

### Navegação ocorrendo antes de waitForNavigation ser chamado:
Se a navegação ocorrer antes de waitForNavigation() ser chamado, a função não será capaz de detectar a navegação e irá aguardar indefinidamente. Para evitar isso, você deve garantir que waitForNavigation() seja chamado antes de iniciar a ação que causa a navegação.

### Navegação não detectada:
A função waitForNavigation() só detecta navegações que alteram a URL. Se a navegação ocorrer de outra maneira (por exemplo, através do uso da API de Histórico para alterar a URL), waitForNavigation() pode não detectar a navegação. Para esses casos, você pode precisar usar outras funções de espera, como waitForFunction(), para verificar a condição de navegação.

### waitForSelector(selector, options):
Esta função espera até que o seletor apareça na página. Isso pode ser útil se a navegação resultar em um novo elemento sendo adicionado à página. Por exemplo:

	await page.waitForSelector('#my-element');

### waitForXPath(xpath, options):
Similar ao waitForSelector, mas usa XPath em vez de CSS selectors. Por exemplo:

	await page.waitForXPath('//p[@class="my-class"]');

### waitForFunction(pageFunction, options, ...args):
Esta função espera até que a pageFunction passada como argumento seja verdadeira. Esta função pode ser usada para verificar uma ampla variedade de condições. Por exemplo, para esperar até que um elemento específico contenha um determinado texto, você poderia fazer:

	await page.waitForFunction(
	  'document.querySelector("body").innerText.includes("my text")'
	);

Essas funções não detectam a navegação por si só, mas podem ser usadas para verificar as alterações que ocorrem como resultado da navegação. Portanto, a estratégia de espera correta a ser usada dependerá de cada caso


	
	

