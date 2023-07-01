<template>
    <div class="container mx-auto p-4">
      <div class="flex items-center justify-center">
        <div class="w-full max-w-md">
          <div class="mb-8">
            <progress class="w-full" :value="currentStep" max="3"></progress>
            <div class="text-center">
              <span class="text-gray-600">{{ currentStep }}/3</span>
            </div>
          </div>
  
          <div v-show="currentStep === 1" class="step-container">
            <h2 class="text-2xl font-bold mb-4">Paso 1</h2>
            <div class="mb-4">
              <label class="block mb-2">País</label>
              <select class="form-select" v-model="formData.pais">
                <option value="">Seleccione un país</option>
                <option v-for="pais in paises" :value="pais" :key="pais">
                  {{ pais }}
                </option>
              </select>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Género</label>
              <select class="form-select" v-model="formData.genero">
                <option value="">Selecciona tu género</option>
                <option v-for="genero in generos" :value="genero" :key="genero">
                  {{ genero }}
                </option>
              </select>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Nombre</label>
              <input class="form-input" type="text" v-model="formData.nombre" />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Apellido</label>
              <input class="form-input" type="text" v-model="formData.apellido" />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Fecha de nacimiento</label>
              <input
                class="form-input"
                type="date"
                v-model="formData.fechaDeNacimiento"
                :max="maxDate"
              />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Tipo de documento</label>
              <select class="form-select" v-model="formData.tipoDocumento">
                <option value="">Selecciona tu tipo de documento</option>
                <option
                  v-for="documento in tipoDocumento"
                  :value="documento"
                  :key="documento"
                >
                  {{ documento }}
                </option>
              </select>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Número de documento</label>
              <input
                class="form-input"
                type="text"
                v-model="formData.numeroDocumento"
                @input="validateField('numeroDocumento')"
              />
              <span v-if="fieldErrors.numeroDocumento" class="error-message">{{ fieldErrors.numeroDocumento }}</span>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Foto del documento (frente)</label>
              <input
                class="form-input"
                type="file"
                accept="image/*"
                v-on:change="formData.fotoDocumentoFrente"
              />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Foto del documento (reverso)</label>
              <input
                class="form-input"
                type="file"
                accept="image/*"
                v-on:change="formData.fotoDocumentoReverso"
              />
            </div>
            <div class="mt-8">
              <button class="btn-primary" @click="nextStep">Siguiente</button>
            </div>
          </div>
  
          <div v-show="currentStep === 2" class="step-container">
            <h2 class="text-2xl font-bold mb-4">Paso 2</h2>
            <div class="mb-4">
              <label class="block mb-2">Email</label>
              <input class="form-input" type="email" v-model="formData.email" @input="validateField('email')" />
              <span v-if="fieldErrors.email" class="error-message">{{ fieldErrors.email }}</span>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Contraseña</label>
              <input
                class="form-input"
                type="password"
                v-model="formData.contrasena1"
                @input="validateField('contrasena1')"
              />
              <span v-if="fieldErrors.contrasena1" class="error-message">{{ fieldErrors.contrasena1 }}</span>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Confirmar contraseña</label>
              <input
                class="form-input"
                type="password"
                v-model="formData.contrasena2"
                @input="validateField('contrasena2')"
              />
              <span v-if="fieldErrors.contrasena2" class="error-message">{{ fieldErrors.contrasena2 }}</span>
            </div>
            <div class="mb-4">
              <label class="block mb-2">Telefono</label>
              <input class="form-input" type="text" v-model="formData.telefono" />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Numero de celular</label>
              <input
                class="form-input"
                type="text"
                v-model="formData.numeroCelular"
              />
            </div>
  
            <div class="mt-8">
              <button class="btn-primary" @click="prevStep">Anterior</button>
              <button class="btn-primary ml-4" @click="nextStep">
                Siguiente
              </button>
            </div>
          </div>
  
          <div v-show="currentStep === 3" class="step-container">
            <h2 class="text-2xl font-bold mb-4">Paso 3</h2>
            <div class="mb-4">
              <label class="block mb-2">Direccion</label>
              <input
                class="form-input"
                type="text"
                v-model="formData.direccion"
              />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Ciudad</label>
              <input class="form-input" type="text" v-model="formData.ciudad" />
            </div>
            <div class="mb-4">
              <label class="block mb-2">Código postal</label>
              <input
                class="form-input"
                type="text"
                v-model="formData.codigoPostal"
              />
            </div>
  
            <div class="mt-8">
              <button class="btn-primary" @click="prevStep">Anterior</button>
              <button class="btn-primary ml-4" @click="submitForm">Enviar</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  

<script>
export default {
  data() {
    return {
      currentStep: 2,
      formData: {
        pais: "",
        genero: "",
        nombre: "",
        apellido: "",
        fechaDeNacimiento: "",
        tipoDocumento: "",
        numeroDocumento: "",
        fotoDocumentoFrente: "",
        fotoDocumentoReverso: "",
        email: "",
        contrasena1: "",
        contrasena2: "",
        telefono: "",
        numeroCelular: "",
        direccion: "",
        ciudad: "",
        codigoPostal: "",
      },
      paises: [],
      countriesLoaded: false,
      generos: ["Masculino", "Femenino", "Otro"],
      tipoDocumento: ["CC", "PASAPORTE", "CEDULA DE EXTRANJERIA"],
      fieldErrors: {
        pais: "",
        genero: "",
        nombre: "",
        apellido: "",
        fechaDeNacimiento: "",
        tipoDocumento: "",
        numeroDocumento: "",
        fotoDocumentoFrente: "",
        fotoDocumentoReverso: "",
        email: "",
        contrasena1: "",
        contrasena2: "",
        telefono: "",
        numeroCelular: "",
        direccion: "",
        ciudad: "",
        codigoPostal: "",
      },
    };
  },
  methods: {
    nextStep() {
      if (this.validateCurrentStep()) {
        this.currentStep++;
      }
    },
    prevStep() {
      this.currentStep--;
    },
    validateCurrentStep() {
      switch (this.currentStep) {
        case 1:
          return this.validateStep1();
        case 2:
          return this.validateStep2();
        case 3:
          return this.validateStep3();
        default:
          return true;
      }
    },
    validateStep1() {
      const fields = [
        "pais",
        "genero",
        "nombre",
        "apellido",
        "fechaDeNacimiento",
        "tipoDocumento",
        "numeroDocumento",
      ];
      for (const field of fields) {
        if (!this.formData[field]) {
          window.alert(`El campo ${field} es obligatorio`);
          return false;
        }
      }
      return true;
    },
    validateStep2() {
      if (!this.formData.email) {
        window.alert("El campo Email es obligatorio");
        return false;
      }
      if (!this.formData.contrasena1) {
        window.alert("El campo Contraseña es obligatorio");
        return false;
      }
      if (this.formData.contrasena1 !== this.formData.contrasena2) {
        window.alert("Las contraseñas no coinciden");
        return false;
      }
      if (!this.formData.telefono) {
        window.alert("El campo Teléfono es obligatorio");
        return false;
      }
      if (!this.formData.numeroCelular) {
        window.alert("El campo Número de celular es obligatorio");
        return false;
      }
      return true;
    },

    validateStep3() {
      if (!this.formData.direccion) {
        window.alert("El campo Dirección es obligatorio");
        return false;
      }
      if (!this.formData.ciudad) {
        window.alert("El campo Ciudad es obligatorio");
        return false;
      }
      if (!this.formData.codigoPostal) {
        window.alert("El campo Código postal es obligatorio");
        return false;
      }
      return true;
    },

    validatePhoneInput() {
      // Eliminamos cualquier caracter no numérico
      this.formData.telefono = this.formData.telefono.replace(/\D/g, "");
      this.formData.numeroCelular = this.formData.numeroCelular.replace(
        /\D/g,
        ""
      );
    },

    submitForm() {
      if (this.validateCurrentStep()) {
        // Imprimimos los datos del formulario en la consola
        console.log(this.formData); 
      }
    },
    validateNumberInput() {
      // Eliminamos cualquier caracter no numérico
      this.formData.numeroDocumento = this.formData.numeroDocumento.replace(
        /\D/g,
        ""
      );
    },
    async fetchCountries() {
      try {
        const response = await fetch(
          "https://restcountries.com/v3.1/independent?status=true"
        );
        const data = await response.json();
        // Llenamos el array de paises con los nombres de los países
        this.paises = data.map((country) => country.name.common);
        this.paises
          ? (this.countriesLoaded = true)
          : (this.countriesLoaded = false);
      } catch (error) {
        console.log("Error al obtener la lista de países:", error);
      }
    },

    validateField(fieldName) {
    const fieldValue = this.formData[fieldName];

    switch (fieldName) {
      case 'email':
        if (!fieldValue.includes('@')) {
          this.fieldErrors.email = 'Debe ingresar un correo electrónico válido.';
        } else {
          this.fieldErrors.email = '';
        }
        break;

      case 'contrasena1':
        if (fieldValue.length < 8) {
          this.fieldErrors.contrasena1 = 'La contraseña debe tener al menos 8 caracteres.';
        } else {
          this.fieldErrors.contrasena1 = '';
        }
        break;

      case 'contrasena2':
        if (fieldValue !== this.formData.contrasena1) {
          this.fieldErrors.contrasena2 = 'Las contraseñas no coinciden.';
        } else {
          this.fieldErrors.contrasena2 = '';
        }
        break;

      case 'numeroDocumento':
        if (isNaN(fieldValue)) {
          this.fieldErrors.numeroDocumento = 'El número de documento debe ser numérico.';
        } else {
          this.fieldErrors.numeroDocumento = '';
        }
        break;

      case 'telefono':
        if (!/^\d{7}$/.test(fieldValue)) {
          this.fieldErrors.telefono = 'El número de teléfono debe tener 7 dígitos.';
        } else {
          this.fieldErrors.telefono = '';
        }
        break;

      case 'numeroCelular':
        if (!/^\d{10}$/.test(fieldValue)) {
          this.fieldErrors.numeroCelular = 'El número de celular debe tener 10 dígitos.';
        } else {
          this.fieldErrors.numeroCelular = '';
        }
        break;

      case 'codigoPostal':
        if (!/^\d{5}$/.test(fieldValue)) {
          this.fieldErrors.codigoPostal = 'El código postal debe tener 5 dígitos.';
        } else {
          this.fieldErrors.codigoPostal = '';
        }
        break;

      default:
        break;
    }
  },

  handleFieldInput(fieldName) {
    this.validateField(fieldName);
  },
  },
  computed: {
    maxDate() {
      // Calculamos la fecha máxima permitida
      // para seleccionar en el campo de fecha de nacimiento
      const today = new Date();
      const minDate = new Date();
      // Resta 18 años a la fecha actual
      minDate.setFullYear(today.getFullYear() - 18);
      // Retornamos la fecha en formato ISO (AAAA-MM-DD)
      return minDate.toISOString().split("T")[0];
    },
  },
  mounted() {
    this.fetchCountries();
  },

};
</script>

<style>
.container {
  background-color: #f3f4f6;
}

progress::-webkit-progress-value {
  background-color: hsl(207, 100%, 60%);
}

.step-container {
  border: 1px solid #cbd5e0;
  border-radius: 0.25rem;
  padding: 1rem;
  margin-bottom: 1rem;
}

.btn-primary {
  background-color: hsl(207, 100%, 60%);
  color: #ffffff;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
  transition: background-color 0.3s ease;
}

.btn-primary:hover {
  background-color: hsl(207, 100%, 50%);
}
.error {
  border-color: red; 
}

.error-message {
  color: red; 
  margin-top: 0.5rem;
}
</style>
