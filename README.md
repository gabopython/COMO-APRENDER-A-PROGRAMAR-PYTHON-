# CÓMO APRENDER A PROGRAMAR (PYTHON)
## PASO 1: CONCEPTOS BASICOS
>- IF ELIF ELSE
>- VARIABLES
>- BUCLES
>- LISTAS
>- FUNCIONES

Cursos recomendados:

[HolaMundo](https://www.youtube.com/watch?v=tQZy0U8s9LY&t=2s)

[Fazt](https://www.youtube.com/watch?v=chPhlsHoEPo&t=1s)

[IEEE ITBA](https://colab.research.google.com/drive/1HF2JwuaXPRLjHqmWEyuQdSY8JaEwY12c)

##  PASO 2: PRACTICAR
>- BUSCA [EJERCICOS BÁSICOS](https://pythondiario.com/2013/05/ejercicios-en-python-parte-1.html) Y TRATA DE RESOLVERLOS
>- [HACKERRANK](https://www.hackerrank.com/) (UNA FORMA ELEGANTE DE RESOLVER PROBLEMAS DE PROGRAMACION)
>- PROGRAMA UN [BOT](https://www.youtube.com/watch?v=rl0IvcdTUqI&t=82s) QUE AUTOMATICE ALGUNA TAREA QUE REALIZAS RECURRENTEMENTE
>- DESARROLLA UN [VIDEOJUEGO](https://www.youtube.com/watch?v=jrUJ8EsnctI) CON PYGAME (PYGAME ES TAN FACIL COMO PYTHON, PERO REQUIERE CONCEPTOS INTERMEDIOS COMO [PROGRAMACION ORIENTADA A OBJETOS](https://www.youtube.com/watch?v=pYucmbJ-93w&list=PLVzwufPir356z6MI_zp7w7Cg_t7QT20As))
##  PASO 3: ESPECIALIZARSE
>- QUIERES DESARROLLAR WEBSITES, APRENDE [FLASK](https://www.youtube.com/watch?v=fxavwHPJ36o&t=249s) Y/O [DJANGO](https://www.youtube.com/watch?v=T1intZyhXDU)
>- QUIERES DESARROLLAR AI'S, APRENDE [MACHINE LEARNING](https://www.youtube.com/watch?v=lomJnbN5Wnk&list=PLat2DtY8K7YWG4QxUruT1IB_sHumOr1Si) Y [DEEP LEARNING](https://www.youtube.com/watch?v=ftlqZwb33SE&list=PLWzLQn_hxe6ZlC9-YMt3nN0Eo-ZpOJuXd)
>- ETC



CUANDO APRENDES A PROGRAMAR SIEMPRE VAS A TENER DUDAS Y AUNQUE EN INTERNET HAY TODO, EN OCACINES ES DIFICIL ENCONTRARLO, POR ESO TE RECOMIENDO UNIRTE A MI [GRUPO](https://t.me/pythonprogramming2gr) DE TELEGRAM DONDE NOS APOYAMOS MUTUAMENTE 




APOYA EL CONTENIDO

<div id="smart-button-container">
    <div style="text-align: center"><label for="description">apoya el contenido </label><input type="text" name="descriptionInput" id="description" maxlength="127" value=""></div>
      <p id="descriptionError" style="visibility: hidden; color:red; text-align: center;">Please enter a description</p>
    <div style="text-align: center"><label for="amount"> </label><input name="amountInput" type="number" id="amount" value="" ><span> USD</span></div>
      <p id="priceLabelError" style="visibility: hidden; color:red; text-align: center;">Please enter a price</p>
    <div id="invoiceidDiv" style="text-align: center; display: none;"><label for="invoiceid"> </label><input name="invoiceid" maxlength="127" type="text" id="invoiceid" value="" ></div>
      <p id="invoiceidError" style="visibility: hidden; color:red; text-align: center;">Please enter an Invoice ID</p>
    <div style="text-align: center; margin-top: 0.625rem;" id="paypal-button-container"></div>
  </div>
  <script src="https://www.paypal.com/sdk/js?client-id=sb&enable-funding=venmo&currency=USD" data-sdk-integration-source="button-factory"></script>
  <script>
  function initPayPalButton() {
    var description = document.querySelector('#smart-button-container #description');
    var amount = document.querySelector('#smart-button-container #amount');
    var descriptionError = document.querySelector('#smart-button-container #descriptionError');
    var priceError = document.querySelector('#smart-button-container #priceLabelError');
    var invoiceid = document.querySelector('#smart-button-container #invoiceid');
    var invoiceidError = document.querySelector('#smart-button-container #invoiceidError');
    var invoiceidDiv = document.querySelector('#smart-button-container #invoiceidDiv');

    var elArr = [description, amount];

    if (invoiceidDiv.firstChild.innerHTML.length > 1) {
      invoiceidDiv.style.display = "block";
    }

    var purchase_units = [];
    purchase_units[0] = {};
    purchase_units[0].amount = {};

    function validate(event) {
      return event.value.length > 0;
    }

    paypal.Buttons({
      style: {
        color: 'blue',
        shape: 'rect',
        label: 'paypal',
        layout: 'vertical',
        
      },

      onInit: function (data, actions) {
        actions.disable();

        if(invoiceidDiv.style.display === "block") {
          elArr.push(invoiceid);
        }

        elArr.forEach(function (item) {
          item.addEventListener('keyup', function (event) {
            var result = elArr.every(validate);
            if (result) {
              actions.enable();
            } else {
              actions.disable();
            }
          });
        });
      },

      onClick: function () {
        if (description.value.length < 1) {
          descriptionError.style.visibility = "visible";
        } else {
          descriptionError.style.visibility = "hidden";
        }

        if (amount.value.length < 1) {
          priceError.style.visibility = "visible";
        } else {
          priceError.style.visibility = "hidden";
        }

        if (invoiceid.value.length < 1 && invoiceidDiv.style.display === "block") {
          invoiceidError.style.visibility = "visible";
        } else {
          invoiceidError.style.visibility = "hidden";
        }

        purchase_units[0].description = description.value;
        purchase_units[0].amount.value = amount.value;

        if(invoiceid.value !== '') {
          purchase_units[0].invoice_id = invoiceid.value;
        }
      },

      createOrder: function (data, actions) {
        return actions.order.create({
          purchase_units: purchase_units,
        });
      },

      onApprove: function (data, actions) {
        return actions.order.capture().then(function (orderData) {

          // Full available details
          console.log('Capture result', orderData, JSON.stringify(orderData, null, 2));

          // Show a success message within this page, e.g.
          const element = document.getElementById('paypal-button-container');
          element.innerHTML = '';
          element.innerHTML = '<h3>Thank you for your payment!</h3>';

          // Or go to another URL:  actions.redirect('thank_you.html');
          
        });
      },

      onError: function (err) {
        console.log(err);
      }
    }).render('#paypal-button-container');
  }
  initPayPalButton();
  </script>








