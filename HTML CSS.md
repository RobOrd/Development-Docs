
# Cascading Style Sheets #

### Definición de reglas de estilo a través del selector *class* ###
    <h1 class="boldred">This is a sample text</h1>

    .boldred {
    	font-family: serif;
    	font-size: 20pt;
    	color: red;
    }

### Definición de reglas de estilo a través del selector *id* ###
    <p id="importantInfo">This is another sample text</p>
    
    #importantInfo {
    	color: red;
    	font-size: 14pt;
    }

- El atributo *id* simplemente sirve dar un nombre único al elemento
- Aplicar estilo a traves de el selector *id* significa que la regla aplica única y exclusivamente a ese elemento
- Ejemplos de apliación real de la naturaleza *cascada* de las hojas de estilo
    
    <div id=newspost">
    	<h1>News article 1</h1>
    	<p>The article text for news article one</p>
    	<h1>News article 2</h1>
    	<p>The article text for news article two</p>
    </div>    

    #newspost {
    	background-color: silver;
    }    

    #newspost h1 {
    	color: blue; 
    	font-size: 15pt;
    }

    #newspost p {
    	color: red; 
    	font-size: 10pt; 
    }

- Este ejemplo también se pudo haber aplicado usando el selector *class*

    .newspost {
    	background-color: silver;
    }
    
    .newspost h1 {
    	color: blue; 
    	font-size: 15pt;
    }
    .newspost p {
    	color: red; 
    	font-size: 10pt; 
    }

### Pseudo Selectors ###
Un pseudo selector funciona modificando una regla existente bajo un escenario especifico, por ejemplo el evento *hover*

    <a href="#" class="hoverClass">Hover over me I'll change color</a>
    
    .hoverClass { 
    	font-size: 20pt; 
    	color: red; 
    } 
    
    .hoverClass:hover { 
    	color: blue; 
    }

### Direct Selectors ###
Es usado para seleccionar directamente un elemento conocido a través de su nombre. Por ejemplo 
si definimos la siguiente regla significa que todo elemento *p* dentro de un documento dado sera de color verde 
    
    p { 
    	color: green; 
    }
    




































