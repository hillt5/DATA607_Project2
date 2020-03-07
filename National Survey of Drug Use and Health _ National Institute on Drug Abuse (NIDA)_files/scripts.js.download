function PiiForm(config) {
  "use strict";

  this.config = config !== undefined?config:{
    parentTag: 'body',
    containerTag: 'div',
    attachStylesheet: true,
    path: 'collecting-feedback',
    debug: true
  };

  this.uuid = false;

  this.parentElement = false;
  this.containerElement = false;
  this.closeElement = false;

  this.language = 'en';

  this.params = {
    'site_path': window.location.href,
    'language': this.language
  };

  if(this.params.site_path.search(/\/es\//) > -1) {
    this.language = 'es'
  }

  this.params.language = this.language;

  this.setParentElement();
  this.attachStylesheet();
}

PiiForm.prototype.displayForm = function displayForm(delay) {
  "use strict";

  var http = new XMLHttpRequest(),
    url = "/fbf_api/get_uuid",
    that = this,
    defaultDelay = delay === undefined?30000:delay,
    params = [];

  for(var param in this.params){
    params.push(param + '=' + this.params[param]);
  }

  http.open("POST", url, true);

  //Send the proper header information along with the request
  http.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

  http.onreadystatechange = function() {//Call a function when the state changes.
    if(http.readyState === 4 && http.status === 200) {
      try {

        setTimeout(function() {
          that.buildContainer();
          that.buildInputGroups();
        }, defaultDelay);
      }
      catch (e) {
        that.uuid = false;
      }
    }
  };

  http.send(params.join('&'));
};

PiiForm.prototype.removeForm = function removeForm() {
  "use strict";

  var that = this;

  this.closeElement.setAttribute('style','display:none;');

  setTimeout(function() {
    that.parentElement[0].removeChild(that.containerElement);
  }, 7000);

};

PiiForm.prototype.buildContainer = function buildContainer() {
  "use strict";
  this.closeElement = document.createElement('img');
  var that = this;

  this.containerElement = document.createElement(this.config.containerTag);
  this.containerElement.setAttribute('class','pii-form-container local-override-class');

  this.closeElement.setAttribute('src',this.config.path+'/close-icon.svg');
  this.closeElement.setAttribute('class','pii-form-close');

  this.closeElement.addEventListener('click',function() {
    var d = new Date(),
      expires = 'expires=';
    d.setTime(d.getTime() + (1*24*60*60*1000)); //days*hours*minutes*seconds*milliseconds
    expires += d.toUTCString();
    document.cookie = 'nidafbfclsd=yes;'+expires+";path=/";
    that.parentElement[0].removeChild(that.containerElement);
  });

  this.containerElement.appendChild(this.closeElement);

  this.parentElement[0].appendChild(this.containerElement);
};

PiiForm.prototype.attachToParent = function attachToParent() {
  "use strict";

  this.parentElement[0].appendChild(this.containerElement);
};

PiiForm.prototype.setParentElement = function setParentElement() {
  "use strict";

  this.parentElement = document.getElementsByTagName(this.config.parentTag);
};

PiiForm.prototype.attachStylesheet = function attachStylesheet() {
  "use strict";

  if(this.config.attachStylesheet) {
    var stylesheet = document.createElement('link');

    stylesheet.setAttribute('rel','stylesheet');
    stylesheet.setAttribute('href',this.config.path+'/dist/styles.css');

    this.parentElement[0].appendChild(stylesheet);
  }

};

PiiForm.prototype.generateForm = function generateForm(config, input) {
  "use strict";

  var inputGroupElement = document.createElement('div'),
    inputGroupLabel = document.createElement('label');

  inputGroupElement.setAttribute('class', 'pii-form'+ (config.active?' active':''));
  inputGroupElement.setAttribute('id', config.id + '_container');

  inputGroupLabel.setAttribute('class', 'pii-form-label');
  inputGroupLabel.innerHTML = config.text[this.language];

  inputGroupElement.appendChild(inputGroupLabel);
  inputGroupElement.appendChild(input);

  return inputGroupElement;

};

PiiForm.prototype.generateTextareaForm = function generateTextareaForm(config) {
  "use strict";

  var inputGroupInput = document.createElement('textarea'),
    inputGroupElement,
    inputGroupButton = document.createElement('button'),
    that = this;

  inputGroupInput.setAttribute('id', config.id);
  inputGroupInput.setAttribute('name', config.id);
  inputGroupInput.setAttribute('placeholder', config.placeholder[this.language]);

  if(this.language === 'es') {
    inputGroupButton.innerHTML = "Continuar";
  } else {
    inputGroupButton.innerHTML = "Continue";
  }

  inputGroupElement = this.generateForm(config,inputGroupInput);
  inputGroupElement.appendChild(inputGroupButton);

  inputGroupButton.addEventListener('click',function(event) {
    that.submitGroup(inputGroupInput);
  });

  return inputGroupElement;

};

PiiForm.prototype.generateSelectForm = function generateSelectForm(config) {
  "use strict";

  var inputGroupElement,
    inputGroupInput = document.createElement('select'),
    option,
    that = this;

  inputGroupElement = this.generateForm(config,inputGroupInput);

  inputGroupInput.setAttribute('id', config.id);
  inputGroupInput.setAttribute('name', config.id);

  config.selectOptions[this.language].forEach(function(selectOptions){

    option = document.createElement('option');
    option.value = option.textContent = selectOptions;

    inputGroupInput.appendChild(option);

  });

  inputGroupInput.addEventListener('change',function() {
    that.submitGroup(this);
  });

  return inputGroupElement;

};

PiiForm.prototype.generateRadioForm = function generateRadioForm(config) {
  "use strict";

  var inputGroupElement,
    radioGroup = document.createElement('div');

  radioGroup.setAttribute('class', 'pii-form-radio-group');

  for(i in config.options) {
    this.generateRadioGroup(config.options[i], radioGroup);
  }

  inputGroupElement = this.generateForm(config,radioGroup);

  return inputGroupElement;

}

PiiForm.prototype.generateRadioGroup = function generateRadioGroup(config,radioGroup) {
  "use strict";

  var inputGroupInputLabel = document.createElement('label'),
    inputGroupInput = document.createElement('input'),
    that = this;

  inputGroupInputLabel.setAttribute('for', config.id);
  inputGroupInputLabel.setAttribute('class', 'pii-form-radio' + config.class);
  inputGroupInputLabel.innerHTML = config.text[this.language];
  inputGroupInput.setAttribute('type', 'radio');
  inputGroupInput.setAttribute('id', config.id);
  inputGroupInput.setAttribute('name', config.name);
  inputGroupInput.setAttribute('value', config.value);

  radioGroup.appendChild(inputGroupInput);
  radioGroup.appendChild(inputGroupInputLabel);

  inputGroupInput.addEventListener('click',function() {
    that.submitGroup(this);
  });

};

PiiForm.prototype.buildInputGroups = function buildInputGroups() {
  "use strict";

  var wasHelpfulForm,
    whyNotForm,
    wouldLikeFeedback,
    selectOptionForm,
    question_fiveForm;
    //ADD HERE;

  wasHelpfulForm = this.generateRadioForm({
    active: true,
    id: 'washelpful',
    text: {
      en: 'Was this article helpful?',
      es: '¿Este artículo le resultó útil?'
    },
    options: [{
      id: 'yeshelpful',
      text: {
        en: 'Yes',
        es: 'Sí'
      },
      name: 'washelpful',
      value: 'yes',
      class: ' pii-form-radio-yes'
    },
      {
        id: 'nothelpful',
        text: {
          en: 'No',
          es: 'No'
        },
        name: 'washelpful',
        value: 'no',
        class: ' pii-form-radio-no'
      }]
  });

  this.containerElement.appendChild(wasHelpfulForm);

  wouldLikeFeedback = this.generateTextareaForm({
    active: false,
    id: 'lookingfor',
    text: {
      en: "We'd appreciate your feedback!",
      es: 'Agradeceremos sus comentarios.'
    },
    placeholder: {
      en: 'We welcome your feedback.',
      es: 'Nos gustaría recibir sus comentarios.'
    }
  });

  this.containerElement.appendChild(wouldLikeFeedback);

  whyNotForm = this.generateTextareaForm({
    active: false,
    id: 'whynot',
    text: {
      en: "Can you give us more details?",
      es: '¿Puede darnos más detalles?'
    },
    placeholder: {
      en: 'We welcome your feedback.',
      es: 'Nos gustaría recibir sus comentarios.'
    }
  });

  this.containerElement.appendChild(whyNotForm);

  selectOptionForm = this.generateSelectForm({
    active: false,
    id: 'selectprofession',
    text: {
      en: "To help us write better content for others like you, can you tell us what best describes you?",
      es: 'Para ayudarnos a escribir un mejor contenido para otras personas como usted, ¿puede decirnos qué es lo que mejor lo describe?'
    },
    selectOptions: {
      en: [
        'Choose one?',
        'Counselor/Social Worker',
        'Criminal Justice Professional',
        'Healthcare Professional',
        'Public Health Professional',
        'Scientist',
        'Teacher/Educator',
        'Military Personnel/Family',
        'Parent/Caregiver',
        'Concerned Friend/Relative',
        'Student, Elementary or Middle School',
        'Student, High School',
        'Student, College/Grad',
        'Other' ],
      es: [
        '¿Cómo se describiría usted?',
        'Educador',
        'Estudiante universitario o de posgrado',
        'Parte de las fuerzas militares',
        'Empleado del gobierno',
        'Profesional de justicia penal',
        'Consejero',
        'Profesional de la salud',
        'Estudiante de la escuela secundaria (“high school”)',
        'Estudiante del jardín de infantes al 12º grado (“K-12”)',
        'Padre de familia/Persona que cuida a alguien',
        'Investigador',
        'Amigo o familiar interesado',
        'Otro' ]
    }
  });

  this.containerElement.appendChild(selectOptionForm);


    question_fiveForm = this.generateSelectForm({
        active: false,
        id: 'question_five',
        text: {
            en: "How often have you visited this site in the last 6 months [or 12 months]?",
            es: ''
        },
        selectOptions: {
            en: [
                'Choose one',
                'This is my first visit.',
                '1-2 times',
                '3-5 times.',
                '5+ times.'
                 ],
            es: [
                 ]
        }
    });

    this.containerElement.appendChild(question_fiveForm);

  var inputGroupElement = document.createElement('div');
  var inputGroupLabel = document.createElement('h2');
  var timerContainer = document.createElement('p');

  inputGroupElement.setAttribute('class', 'pii-form');
  inputGroupElement.setAttribute('id', 'thankyou_container');
  timerContainer.setAttribute('id', 'fbftimer');
  timerContainer.setAttribute('class', 'fbftimer');

  inputGroupLabel.setAttribute('class', 'pii-form-label');
  if(this.language === 'es'){
    inputGroupLabel.innerHTML = "¡Gracias por tus comentarios!";
    timerContainer.innerHTML = "Este formulario se cerrará en 7 segundos";
  } else{
    inputGroupLabel.innerHTML = "Thank you for your feedback!";
    timerContainer.innerHTML = "This form will close in 7 seconds";
  }

  inputGroupElement.appendChild(inputGroupLabel);
  inputGroupElement.appendChild(timerContainer);

  this.containerElement.appendChild(inputGroupElement);

};

PiiForm.prototype.submitGroup = function submitGroup(input) {
  "use strict";

  var previousContainer;

  this.params[input.name] = input.value;
  previousContainer = input.name+'_container';

  document.getElementById(previousContainer).classList.toggle('active');

  switch(input.name) {
    case 'lookingfor':
      document.getElementById('thankyou_container').classList.toggle('active');

      this.sendData();
      this.removeForm();
      break;
    case 'whynot':
      document.getElementById('selectprofession_container').classList.toggle('active');

      break;
    case 'selectprofession':
        document.getElementById('question_five_container').classList.toggle('active');

      break;
    case 'question_five':
        if(this.params['washelpful'] === 'yes') {
            document.getElementById('lookingfor_container').classList.toggle('active');
        }
        else {
            document.getElementById('thankyou_container').classList.toggle('active');
            this.sendData();
            this.removeForm();
        }
        break;
    case 'washelpful':

      if(input.value === 'yes') {
        document.getElementById('selectprofession_container').classList.toggle('active');
      }
      else if(input.value === 'no') {
        document.getElementById('whynot_container').classList.toggle('active');
      }

      break;
  }
};

PiiForm.prototype.sendData = function sendData() {
  "use strict";

  var http = new XMLHttpRequest(),
    url = "/fbf_api/submit_form",
    params = [];

  for(var param in this.params){
    params.push(param + '=' + this.params[param]);
  }

  http.open("POST", url, true);

  //Send the proper header information along with the request
  http.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

  http.send(params.join('&'));
};
//# sourceMappingURL=scripts.js.map
