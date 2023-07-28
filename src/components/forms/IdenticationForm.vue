<template>
  <h2 class="title">Identificación</h2>
  <div class="container-fluid mt-5">
    <form class="row g-3">
      <div class="col-md-4">
        <label for="inputName" class="form-label">Nombre*</label>
        <input
          type="text"
          class="form-control"
          v-model="state.name"
          id="user-name"
        />
        <span v-if="v$.name.$error">
          {{ v$.name.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-4">
        <label for="inputPassword4" class="form-label">Primer apellido*</label>
        <input
          type="text"
          class="form-control"
          v-model="state.firstSurname"
          id="firstSurname"
        />
        <span v-if="v$.firstSurname.$error">
          {{ v$.firstSurname.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-4">
        <label class="form-label">Segundo apellido</label>
        <input
          type="text"
          class="form-control"
          v-model="state.secondLastName"
          id="secondLastName"
        />
        <span v-if="v$.secondLastName.$error">
          {{ v$.state.secondLastName.$errors[0].$message }}
        </span>
      </div>
      <div class="col-6">
        <label for="inputAddress" class="form-label">CURP*</label>
        <input
          type="text"
          class="form-control"
          id="curp"
          v-model="state.curp"
        />
        <span v-if="v$.curp.$error">
          {{ v$.curp.$errors[0].$message }}
        </span>
      </div>
      <div class="col-6">
        <label for="inputAddress2" class="form-label"
          >RFC (con homoclave)*</label
        >
        <input type="text" class="form-control" id="rfc" v-model="state.rfc" />
        <span v-if="v$.rfc.$error">
          {{ v$.rfc.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-6">
        <label for="inputCity" class="form-label">Código*</label>
        <input
          type="number"
          class="form-control"
          v-model="state.zipCode"
          id="input-zipCode"
        />
        <span v-if="v$.zipCode.$error">
          {{ v$.zipCode.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-6">
        <label for="inputState" class="form-label">Calle*</label>
        <input
          type="text"
          class="form-control"
          v-model="state.streetName"
          id="input-streetName"
        />
        <span v-if="v$.streetName.$error">
          {{ v$.streetName.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-4">
        <label for="inputExteriorNumber" class="form-label"
          >Número exterior*</label
        >
        <input
          type="text"
          class="form-control"
          v-model="state.exteriorNumber"
          id="input-exterior-number"
        />
        <span v-if="v$.exteriorNumber.$error">
          {{ v$.exteriorNumber.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-4">
        <label for="inputInteriorNumber" class="form-label"
          >Número interior</label
        >
        <input
          type="text"
          class="form-control"
          v-model="state.interiorNumber"
          id="input-interior-number"
        />
        <span v-if="v$.interiorNumber.$error">
          {{ v$.interiorNumber.$errors[0].$message }}
        </span>
      </div>
      <div class="col-md-4">
        <label for="inputExteriorNumber" class="form-label">Estado*</label>
        <select
          id="inputState"
          v-model="state.mexicoStates"
          class="form-select"
        >
          
          <option v-for="mexState in mexStates" >{{ mexState.name }}</option>
        </select>
        <span v-if="v$.mexicoStates.$error">
          {{ v$.mexicoStates.$errors[0].$message }}
        </span>
      </div>

      <div class="col-md-6">
        <label for="inputTown" class="form-label"
          >Delegación / Municipio*</label
        >
        <input
          type="text"
          class="form-control"
          v-model="state.town"
          id="input-town"
        />
        <span v-if="v$.town.$error">
          {{ v$.town.$errors[0].$message }}
        </span>
      </div>

      <div class="col-md-6">
        <label for="inputColony" class="form-label">Colonia*</label>
        <input
          type="text"
          class="form-control"
          v-model="state.colony"
          id="input-colony"
        />
        <span v-if="v$.colony.$error">
          {{ v$.colony.$errors[0].$message }}
        </span>
      </div>
      <div class="col-12">
        <button
          type="submit"
          @click="submitForm($event, v$, state)"
          class="btn btn-primary btn-sumbit-form"
        >
          Enviar
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import { mexico_states } from '../../data/mexico_states'
import Swal from "sweetalert2";
import { useVuelidate } from "@vuelidate/core";
import {
  required,
  helpers,
  between,
  maxLength,
  minLength,
  alphaNum,
  numeric,
  alpha,
} from "@vuelidate/validators";
import { reactive, computed } from "vue";

const mexStates = mexico_states.states
export default {
  setup() {
    const state = reactive({
      name: "",
      firstSurname: "",
      secondLastName: "",
      curp: "",
      rfc: "",
      zipCode: "",
      streetName: "",
      exteriorNumber: "",
      interiorNumber: "",
      state: "",
      town: "",
      colony: "",
    });

    const rules = computed(() => {
      return {
        name: {
          required: helpers.withMessage("El campo es requerido", required),
          alpha: helpers.withMessage("Solo letras", alpha),
        },
        firstSurname: {
          required: helpers.withMessage("El campo es requerido", required),
          alpha: helpers.withMessage("Solo letras", alpha),
        },
        secondLastName: { alpha: helpers.withMessage("Solo letras", alpha) },
        curp: {
          required: helpers.withMessage("El campo es requerido", required),
          minLength: helpers.withMessage("Minimo 18 caracteres", minLength(18)),
          maxLength: helpers.withMessage("Maximo 18 caracteres", maxLength(18)),
          alphaNum,
        },
        rfc: {
          required: helpers.withMessage("El campo es requerido", required),
          minLength: helpers.withMessage("Minimo 13 caracteres", minLength(13)),
          maxLength: helpers.withMessage("Maximo 13 caracteres", maxLength(13)),
          alphaNum,
        },
        zipCode: {
          required: helpers.withMessage("El campo es requerido", required),
          numeric: helpers.withMessage("Solo números", numeric),
          minLength: helpers.withMessage("Minimo 5 caracteres", minLength(5)),
          maxLength: helpers.withMessage("Maximo 5 caracteres", maxLength(5)),
        },
        streetName: {
          required: helpers.withMessage("El campo es requerido", required),
          alpha: helpers.withMessage("Solo letras", alpha),
        },
        exteriorNumber: {
          required: helpers.withMessage("El campo es requerido", required),
          numeric: helpers.withMessage("Solo números", numeric),
        },
        interiorNumber: {
          numeric: helpers.withMessage("Solo números", numeric),
          maxLength: helpers.withMessage("Maximo 10 caracteres", maxLength(10)),
        },
        mexicoStates: {
          required: helpers.withMessage("El campo es requerido", required),
          alpha: helpers.withMessage("Solo letras", alpha),
        },
        town: {
          rrequired: helpers.withMessage("El campo es requerido", required),
          alpha: helpers.withMessage("Solo letras", alpha),
        },
        colony: {
          required: helpers.withMessage("El campo es requerido", required),
          alpha: helpers.withMessage("Solo letras", alpha),
        },
      };
    });

    const v$ = useVuelidate(rules, state);

    return {
      state,
      v$,
      mexStates
    };
  },

  methods: {
    submitForm(e) {
      this.v$.$validate();
      e.preventDefault();
      console.log(this.state);
      if (!this.v$.$error) {
        fetch("http://httpbin.org/post", {
          method: "POST",
          body: JSON.stringify({
            infoUsuario: {
              Nombre: this.state.name,
              Primer_Apellido: this.state.firstSurname,
              Segundo_Apellido: this.state.secondLastName,
              Curp: this.state.curp,
              Rfc: this.state.rfc,
            },
            Domicilio: {
              Calle: this.state.streetName,
              Estado: this.state.mexicoStates,
              Municipio: this.state.town,
              Colonia: this.state.colony,
              CodigoPostal: this.state.zipCode,
              NumeroExterior: this.state.exteriorNumber,
              NumeroInterior: this.state.interiorNumber,
            },
          }),
          headers: {
            "Content-Type": "application/json",
          },
        })
        .then((response) => response.json())
        .then((data) => {
          console.log("-------------- Success --------------")
          console.log(data)
          Swal.fire({
              title: "Formulario enviado",
              text: "Campos validados correctamente.",
              icon: "success",
              confirmButtonText: "OK",
            });
        })
        .catch((error) => console.error("Error en la peticion: " + error));
      } else {
        Swal.fire({
          title: "Upss... Error en el formulario",
          text: "Existen campos por validar.",
          icon: "warning",
          confirmButtonText: "OK",
        });
      }
    },
  },
};
</script>
<style scoped>
.title {
  text-align: center;
  margin-top: 50px;
}

.form-label{
  color: #35d4c7;
  font-weight: 500;
}
.btn-sumbit-form {
  width: 10%;
  margin: 8px 45%;
}

@media screen and (min-width: 415px) and (max-width: 600px) {
    .form-group {
        min-width: 300px !important;
    }

    .btn-submit-form {
        margin: 0 36%;
    }
}

@media screen and (max-width: 414px) {
    .form-group {
        min-width: 300px !important;
    }

    .btn-submit-form {
        margin: 0 36%;
    }
}
</style>
