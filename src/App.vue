<template>
  <div>
    <header class="header"
      :style="{ backgroundColor: colorheader === '1' ? '#DEB71C' : colorheader === '2' ? '#7F6AEC' : colorheader === '3' ? '#FB4474' : colorheader === '4' ? '#FC8811' : '#B6D989' }">
      <h2 class="titulo-header">TALONARIO</h2>
    </header>
    <main>
      <div class="modal-intro" v-if="modal_intro">
        <div class="intro">
          <div class="color-titulo"
            :style="{ backgroundColor: colorheader === '1' ? '#DEB71C' : colorheader === '2' ? '#7F6AEC' : colorheader === '3' ? '#FB4474' : colorheader === '4' ? '#FC8811' : '#B6D989' }">
            <span class="closeicon2" @click="aviso()"><i class="fa fa-times"></i></span>
            <h2 class="titulo-info-talonario">CONFIGURA TU TALONARIO</h2>
          </div>
          <div class="input-group">
            <input type="tel" placeholder="Ingrese el premio de la rifa" v-model="vrifa">
            <input type="tel" placeholder="Ingrese valor de la boleta" v-model="vboleta">
            <select name="" id="" v-model="loterias">
              <option disabled selected hidden value="">Seleccione la loteria </option>
              <option value="Cruz Roja">Cruz Roja</option>
              <option value="La Perla">La Perla</option>
              <option value="Santander">Santander</option>
              <option value="Baloto">Baloto</option>
            </select>
            <select name="" id="" v-model="cantboletas" :disabled="selectDeshabilitado">
              <option disabled selected hidden value="">Cantidad de Boletas</option>
              <option value="100">0-99</option>
              <option value="1000">0-999</option>
            </select>
            <input type="date" placeholder="Fecha de sorteo" v-model="fecha">
            <button class="buttonr" @click="validar()"
              :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">Guardar</button>
          </div>

        </div>
      </div>
      <div class="contenedor">
        <div class="cont-informacion">
          <h3 class="titulo-info">Información del Talonario</h3>
          <div class="cuerpo-info" v-for="(item, i) in datostalonario" :key="i">
            <p class="icon">🏆<span>{{ item.vrifa }}</span></p>
            <p class="icon">💲<span>{{ item.vboleta }}</span></p>
            <p class="icon">🏦<span>{{ item.loterias }}</span></p>
            <p class="icon">🗓️<span>{{ item.fecha }}</span></p>
            <h6></h6>
            <div class="boton">
              <button class="btn-editar" @click="editar(item, i)"
                :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">
                Editar<i class="fa fa-edit"></i></button>
            </div>
          </div>
        </div>

        <div class="grid">
          <div class="cont-cantboletas">
            <div v-for="(item, i) in arr" :key="i">
              <button class="rounded-circle boleta" :style="item.estado === 0 ? 'background-color: #B6D989'
                : item.estado === 1 ? 'background-color: #CF1635'
                  : item.estado === 2 ? 'background-color: #18BF67'
                    : item.estado === 3 ? 'background-color: #BA8C34' : ''" type="button" data-bs-toggle="modal"
                data-bs-target="#exampleModal" @click="traerDatos(item, i)">{{ item.numero }}</button>
            </div>
          </div>
        </div>
        <div class="cont-acciones">
          <h3 class="titulo-info">Acciones</h3>
          <div class="cuerpo-acciones">
            <button class="btn-acciones" @click="listardatos()"
              :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }"><i
                class="fa fa-list-ul"></i>LISTAR BOLETAS</button>
            <button class="btn-acciones" @click="aparecerpersonalizar()"
              :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }"><i
                class="fa fa-cogs"></i>PERSONALIZAR</button>

          </div>
        </div>
      </div>
      <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered modal-bottom">
          <div class="modal-content">

            <div class="modal-body">
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              <h3>Boleta {{ numsele }}</h3>
              <div v-if="estado === 0" class="cen">
                <h6 class="rowc">Estado: Disponible<div class="color" style="background-color: #B6D989;"></div>
                </h6>
                <hr v-if="estado === 0">
                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#staticBackdrop2"
                  :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">🤝
                  Adquirir Boleta </button>
              </div>
              <div v-if="estado === 1">
                <h6 class="rowc">Estado: Apartado <div class="color" style="background-color: #CF1635;"></div>
                </h6>
              </div>
              <div v-if="estado === 2">
                <h6 class="rowc">Estado: Pagado <div class="color" style="background-color: #18BF67;"></div>
                </h6>
              </div>
              <div v-if="estado === 3">
                <h6 class="rowc">Estado: Ganador <div class="color" style="background-color: #BA8C34;"></div>
                </h6>
              </div>
              <div v-if="estado !== 0" class="cen">
                <hr>
                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#participante"
                  :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">👁‍🗨
                  Ver datos del participante </button>
                <br v-if="estado !== 3">
                <button v-if="estado !== 3" type="button" class="btn btn-primary" @click="libb()"
                  :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">❌
                  Liberar Boleta
                </button>
                <br v-if="estado !== 2 && estado !== 3">
                <button v-if="estado === 1" type="button" class="btn btn-primary" @click="pagar()"
                  :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">
                  ✔ Pagar Boleta
                </button>
                <br v-if="estado === 2">
                <button v-if="estado === 2" type="button" class="btn btn-primary" @click="ganador()"
                  :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">👑
                  Elejir como ganador
                </button>
              </div>

            </div>
          </div>
        </div>
      </div>
      <div class="modal fade prueba-color" id="staticBackdrop2" data-bs-backdrop="static" data-bs-keyboard="false"
        tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content contenido">
            <div class="modal-header ">
              <h1 class="modal-title fs-5 titulo-datos-boleta" id="staticBackdropLabel">Datos de Boleta {{ numsele }}</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <input type="text" placeholder="Ingrese nombre del comprador" v-model="nombreC">
              <input type="text" placeholder="Ingrese direccion del comprador" v-model="direccionC">
              <input type="tel" required pattern="[0-9]+" maxlength="10" placeholder="Ingrese numero telefonico "
                v-model="telefonoC">
              <select v-model="estadoC">
                <option disabled selected hidden value="">Seleccione el estado de la boleta</option>
                <option value="1">Apartado</option>
                <option value="2">Pagado</option>
              </select>
              <button class="btn btn-primary botoncito" @click="validarcliente()"
                :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">Registrar</button>
            </div>
          </div>
        </div>
      </div>

      <div class="modal fade" id="participante" tabindex="-1" aria-labelledby="exampleModalLabel2" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered modal-bottom">
          <div class="modal-content">
            <div class="modal-header  encabezado"
              :style="{ backgroundColor: colorheader === '1' ? '#DEB71C' : colorheader === '2' ? '#7F6AEC' : colorheader === '3' ? '#FB4474' : colorheader === '4' ? '#FC8811' : '#B6D989' }">
              <h1 class="modal-title fs-5 titulo-datos-boleta titulo4" id="exampleModalLabel2">Participante</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <div class="cuerpo">
                <div class="nombre_cliente">
                  <i class="fa fa-user  icon1"></i>
                  <div class="c1">
                    <h6 class="mini">Nombre:</h6>
                    <span>{{ nombreP }}</span>
                  </div>
                </div>
                <div class="direccion_cliente">
                  <i class="fa fa-home icon4"></i>
                  <div class="c1">

                    <h6 class="mini">Dirección:</h6>
                    <span>{{ direccionP }}</span>
                  </div>
                </div>
                <div class="telefono_cliente">
                  <i class="fa fa-phone icon2"></i>
                  <div class="c1">
                    <h6 class="mini">Telefono:</h6>
                    <span>{{ telefonoP }}</span>
                  </div>
                </div>
                <div class="fechas_cliente">
                  <i class="fa fa-calendar icon3"></i>
                  <div class="c1">
                    <h6 class="mini">Fecha:</h6>
                    <span>{{ fechaP }}</span>
                  </div>
                </div>
              </div>
              <div class="estados">
                <i class="fa fa-list icon5"></i>
                <div class="c1">
                  <h6 class="mini">Estado:</h6>
                  <span v-if="estado === 1">Sin Pagar</span>
                  <span v-if="estado === 2">Pagado</span>
                  <span v-if="estado === 3">Ganador</span>
                </div>
                <button data-bs-toggle="modal" data-bs-target="#participanteEdit" class="btn-editar2" :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }" >Editar</button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="listar-boletas caja" v-if="divaparecer2">
        <div class="cont-listar">
          <div class="cont-conted">
            <div class="cont-listado-titulo"
              :style="{ backgroundColor: colorheader === '1' ? '#DEB71C' : colorheader === '2' ? '#7F6AEC' : colorheader === '3' ? '#FB4474' : colorheader === '4' ? '#FC8811' : '#B6D989' }">
              <span class="closeicon " @click="cerrar2()"><i class="fa fa-times"></i></span>
              <h2>Listado de Boleta</h2>
            </div>
            <div class="cont-listado-cuerpo">
              <table id="tab">
                <thead>
                  <tr>
                    <th>Nombre Comprador</th>
                    <th>Dirrecion</th>
                    <th>Numero Telefonico</th>
                    <th>Fecha Compra Boleta</th>
                    <th>Estado Boleta</th>
                    <th>N. Boleta </th>
                   
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, i) in registros" :key="i">
                    <td>{{ item.nombre }}</td>
                    <td>{{ item.direccion }}</td>
                    <td>{{ item.telefono }}</td>
                    <td>{{ item.fecha }}</td>
                    <td>{{ item.estadoTexto }}</td>
                    <td>{{ item.boleta }}</td>
                  </tr>
                  <tr>
                    <td id="r" colspan="6">Dinero recaudado: {{ vpagada }}</td>
                  </tr>
                  <tr>
                    <td id="r" colspan="6">Deuda total: {{ vdeuda }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="contenido2">
              <button class="btn-acciones" @click="download()"
                :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }"><i
                  class="fa fa-download"></i>GENERAR ARCHIVO</button>
            </div>
          </div>
        </div>
      </div>
      <div class="modal-temas" v-if="divaparecer">
        <div class="cont-temas">
          <div class="cont-conte2">
            <div class="titulo_temas"
              :style="{ backgroundColor: colorheader === '1' ? '#DEB71C' : colorheader === '2' ? '#7F6AEC' : colorheader === '3' ? '#FB4474' : colorheader === '4' ? '#FC8811' : '#B6D989' }">
              <span class="closeicon " @click="cerrar()"><i class="fa fa-times"></i></span>
              <h2>Temas Para El Talonario</h2>
            </div>
            <div class="cont-cuerpo">
              <div class="colores_header">
                <div class="titulo_temas2">
                  <h2>Colores Header o Encabezados</h2>
                </div>
                <div class="cont_color_header">
                  <div class="cont_header_amarillo bordercito">
                    <input type="radio" name="color" value="1" v-model.number="colorheader" id="color_amarillo">
                    <div class="cont-color">
                      <label for="color_amarillo">Color Amarillo</label>
                      <div class="color amarillo"></div>
                    </div>
                  </div>
                  <div class="cont_header_verde bordercito">
                    <input type="radio" name="color" value="2" v-model="colorheader" id="color_verde">
                    <div class="cont-color">
                      <label for="color_verde">Color Morado</label>
                      <div class="color verde"></div>
                    </div>
                  </div>
                  <div class="cont_header_rosa bordercito">
                    <input type="radio" name="color" value="3" v-model="colorheader" id="color_rojo">
                    <div class="cont-color">
                      <label for="color_rojo">Color Fucsia</label>
                      <div class="color rosa"></div>
                    </div>
                  </div>
                  <div class="cont_header_naranja bordercito">
                    <input type="radio" name="color" value="4" v-model="colorheader" id="color_naranja">
                    <div class="cont-color">
                      <label for="color_morado">Color Naranja</label>
                      <div class="color naranja"></div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="colores_footer">
                <div class="titulo_temas2">
                  <h2>Colores Footer</h2>
                </div>
                <div class="cont_color_header">
                  <div class="cont_header_amarillo bordercito">
                    <input type="radio" name="color" value="1" v-model="colorfooter" id="color_amarillo">
                    <div class="cont-color">
                      <label for="color_amarillo">Color Amarillo</label>
                      <div class="color amarillo"></div>
                    </div>
                  </div>
                  <div class="cont_header_verde bordercito">
                    <input type="radio" name="color" value="2" v-model="colorfooter" id="color_verde">
                    <div class="cont-color">
                      <label for="color_verde">Color Morado</label>
                      <div class="color verde"></div>
                    </div>
                  </div>
                  <div class="cont_header_rosa bordercito">
                    <input type="radio" name="color" value="3" v-model="colorfooter" id="color_rojo">
                    <div class="cont-color">
                      <label for="color_rojo">Color Fucsia</label>
                      <div class="color rosa"></div>
                    </div>
                  </div>
                  <div class="cont_header_naranja bordercito">
                    <input type="radio" name="color" value="4" v-model="colorfooter" id="color_naranja">
                    <div class="cont-color">
                      <label for="color_morado">Color Naranja</label>
                      <div class="color naranja"></div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="colores_pestañas">
                <div class="titulo_temas2">
                  <h2>Colores Botones</h2>
                </div>
                <div class="cont_color_header">
                  <div class="cont_header_amarillo bordercito">
                    <input type="radio" name="color" value="1" v-model="colorbotones" id="color_amarillo">
                    <div class="cont-color">
                      <label for="color_amarillo">Color Amarillo</label>
                      <div class="color amarillo"></div>
                    </div>
                  </div>
                  <div class="cont_header_verde bordercito">
                    <input type="radio" name="color" value="2" v-model="colorbotones" id="color_verde">
                    <div class="cont-color">
                      <label for="color_verde">Color Morado</label>
                      <div class="color verde"></div>
                    </div>
                  </div>
                  <div class="cont_header_rosa bordercito">
                    <input type="radio" name="color" value="3" v-model="colorbotones" id="color_rojo">
                    <div class="cont-color">
                      <label for="color_rojo">Color Fucsia</label>
                      <div class="color rosa"></div>
                    </div>
                  </div>
                  <div class="cont_header_naranja bordercito">
                    <input type="radio" name="color" value="4" v-model="colorbotones" id="color_naranja">
                    <div class="cont-color">
                      <label for="color_morado">Color Naranja</label>
                      <div class="color naranja"></div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="botones">
                <button class="btn-color" @click="restaurarColor()">Colores por Defecto</button>
                <button class="btn-closed" @click="cerrar()">Cerrar</button>
              </div>

            </div>
          </div>
        </div>
      </div>
      <div class="modal fade prueba-color" id="participanteEdit" data-bs-backdrop="static" data-bs-keyboard="false"
        tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content contenido3">
            <div class="modal-header ">
              <h1 class="modal-title fs-5 titulo-datos-boleta2" id="staticBackdropLabel">Datos del participante</h1>
              <button type="button" class="btn-close  b2" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body2">
              <input type="text" placeholder="Ingrese nombre del comprador" v-model="nombreP">
              <input type="text" placeholder="Ingrese direccion del comprador" v-model="direccionP">
              <input type="tel" required pattern="[0-9]+" maxlength="10" placeholder="Ingrese numero telefonico "
                v-model="telefonoP">
              <button class="btn btn-primary botoncito" @click="editarParticipante()"
                :style="{ backgroundColor: colorbotones === '1' ? '#DEB71C' : colorbotones === '2' ? '#7F6AEC' : colorbotones === '3' ? '#FB4474' : colorbotones === '4' ? '#FC8811' : '#B6D989' }">Editar</button>
            </div>
          </div>
        </div>
      </div>
    </main>
    <footer class="footer"
      :style="{ backgroundColor: colorfooter === '1' ? '#DEB71C' : colorfooter === '2' ? '#7F6AEC' : colorfooter === '3' ? '#FB4474' : colorfooter === '4' ? '#FC8811' : '#B6D989' }">
      <div>
        <p>Copyright ©2025 Derechos reservados Nela Durán.</p>
      </div>
    </footer>
  </div>
</template>


<script setup>
import { ref } from "vue"
import jsPDF from 'jspdf';
import autoTable from 'jspdf-autotable';

let registros = ref([]);
let arr = ref([]);
let colores = ref([]);
let colorheader = ref("");
let colorfooter = ref("");
let colorbotones = ref("")
let datostalonario = ref([]);
let modal_intro = ref(true);
let vrifa = ref("");
let vboleta = ref("");
let loterias = ref("");
let cantboletas = ref("");
let fecha = ref("");
let edit = true;
let index = null;
let i = ref(0);
let numsele = ref(0);
let estado = ref(0);
let nombreC = ref("");
let direccionC = ref("");
let telefonoC = ref("");
let estadoC = ref("");
let nombreP = ref("");
let direccionP = ref("");
let telefonoP = ref("");
let fechaP = ref("");
let estadoP = ref(null)
let estadoTextoP = ref(null)
let divaparecer = ref(false);
let divaparecer2 = ref(false);
let vpagada = ref(0);
let vdeuda = ref(0)
let acum = ref(0)
let acumm = ref(0)
let con = ref(0)
let conn = ref(0)
let selectDeshabilitado = ref(false);
let edit2 = true;
let index2 = null;





function restaurarColor() {
  const color = {
    colorheader: colorheader.value,
    colorfooter: colorfooter.value,
    colorbotones: colorbotones.value

  }
  colores.value.push(color)
  colorheader.value = ""
  colorfooter.value = ""
  colorbotones.value = ""


}

function download() {
  const doc = new jsPDF('landscape');
  let bodyData = [];
  let tableElement = document.getElementById('tab');
  let rows = tableElement.querySelectorAll('tr');

  for (let i = 1; i < rows.length; i++) {
    let row = rows[i];
    let cols = row.querySelectorAll('td');
    let rowData = [];
    for (let j = 0; j < cols.length; j++) {
      let col = cols[j];
      let we = col.innerText
      if (col.id === "r") {
        let rowData = []
        rowData.push({
          content: we,
          colSpan: 6
        })
        bodyData.push(rowData)
      } else {
        rowData.push(col.innerText);
      }
    }
    bodyData.push(rowData);
  }

  let headers = ['Nombre Comprador', 'Direccion', 'Numero Telefonico', 'Fecha Compra Boleta', 'Estado Boleta', 'N. Boleta'];
  autoTable(doc, {
    head: [headers],
    body: bodyData,
    styles: {
      halign: 'center',
    },
  });
  doc.save('Registro.pdf');
}



function validarcliente() {
  let texto = /^[A-Za-zÁÉÍÓÚáéíóúñÑüÜ\s]+$/;

  if (edit2 ==true) {
    if (nombreC.value.trim() == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El nombre del comprador es requerido",
      timer: 3500
    });

  } else if (!texto.test(nombreC.value)) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de nombre comprador no puede llevar numeros",
      timer: 3500
    });
  } else if (direccionC.value.trim() == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "La dirrecion del comprador es requerida",
      timer: 3500
    });
  } else if (telefonoC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El telefono del comprador es requerido",
      timer: 3500
    });
  } else if (isNaN(telefonoC.value) == true) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono del comprador debe ser numerico",
      timer: 3500
    });
  } else if (telefonoC.value.length != 10) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono debe tener al menos 10 numeros",
      timer: 3500
    });
  } else if (estadoC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El estado de la compra de la boleta es requerido",
      timer: 3500
    });
  } else {
    regBoletas()
    limpiar2()

    Swal.fire({
      icon: "success",
      title: "Boleta vendida",
      showConfirmButton: false,
      timer: 1500
    });

    let modal = document.getElementById('staticBackdrop2');
    let bootstrapModal = bootstrap.Modal.getInstance(modal);
    bootstrapModal.hide();
  }
  } else {
    if (nombreC.value.trim() == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El nombre del comprador es requerido",
      timer: 3500
    });

  } else if (!texto.test(nombreC.value)) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de nombre comprador no puede llevar numeros",
      timer: 3500
    });
  } else if (direccionC.value.trim() == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "La dirrecion del comprador es requerida",
      timer: 3500
    });
  } else if (telefonoC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El telefono del comprador es requerido",
      timer: 3500
    });
  } else if (isNaN(telefonoC.value) == true) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono del comprador debe ser numerico",
      timer: 3500
    });
  } else if (telefonoC.value.length != 10) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono debe tener al menos 10 numeros",
      timer: 3500
    });
  } else if (estadoC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El estado de la compra de la boleta es requerido",
      timer: 3500
    });
  } else {
    registros.value[index2].nombre = nombreC.value;
    registros.value[index2].direccion = direccionC.value;
    registros.value[index2].telefono = telefonoC.value;
    limpiar2()

    Swal.fire({
      icon: "success",
      title: "Boleta Actualizada",
      showConfirmButton: false,
      timer: 1500
    });

    let modal = document.getElementById('staticBackdrop2');
    let bootstrapModal = bootstrap.Modal.getInstance(modal);
    bootstrapModal.hide();
  }
  }
}



function regBoletas() {

  const estado = parseInt(estadoC.value);
  let estadoTexto = "";

  if (estado === 1) {
    estadoTexto = "Apartado";
  } else if (estado === 2) {
    estadoTexto = "Pagado";
  } else if (estado === 3) {
    estadoTexto = "Ganador";
  }

  const cliente = {
    nombre: nombreC.value,
    direccion: direccionC.value,
    telefono: telefonoC.value,
    fecha: new Date().toISOString().split('T')[0],
    estado: parseInt(estadoC.value),
    estadoTexto: estadoTexto,
    boleta: numsele.value
  }

  registros.value.push(cliente)
  totalDinero()
  totalDeuda()

  arr.value[i.value].comprador = cliente
  console.log(arr.value);
  arr.value[i.value].estado = parseInt(estadoC.value)


}

function aparecerpersonalizar() {
  divaparecer.value = true

}

function cerrar() {
  divaparecer.value = false

}



function aviso() {
  if (vrifa.value === "" || vboleta.value === ""|| cantboletas.value == "" || loterias.value === "" || fecha.value === "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "Debes completar todos los campos para poder guardar",
      timer: 3500
    });
  } else {
    cerrar3();
  }
}


function cerrar3(){
  modal_intro.value = false

}





function listardatos() {
  divaparecer2.value = true
}

function cerrar2() {
  divaparecer2.value = false

}

function traerDatos(item, index) {
  numsele.value = item.numero
  estado.value = item.estado
  i.value = index

  nombreP.value = item.comprador.nombre
  direccionP.value = item.comprador.direccion
  telefonoP.value = item.comprador.telefono
  fechaP.value = item.comprador.fecha
}

function libb() {


  arr.value[i.value].estado = 0
  arr.value[i.value].comprador = {}

  Swal.fire({
    icon: "success",
    title: "Boleta Liberada",
    showConfirmButton: false,
    timer: 1500
  })

  let modal = document.getElementById('exampleModal');
  let bootstrapModal = bootstrap.Modal.getInstance(modal);
  bootstrapModal.hide();


  let id = numsele.value

  for (let i = 0; i < registros.value.length; i++) {

    if (registros.value[i].boleta === id) {

      registros.value.splice(i, 1)


    }
  }
  if (estado.value === 2) {
    descontar()
  } else {
    descontarD()
  }
}

function pagar() {
  arr.value[i.value].estado = 2


  Swal.fire({
    icon: "success",
    title: "Boleta Pagada",
    showConfirmButton: false,
    timer: 1500
  })

  let modal = document.getElementById('exampleModal');
  let bootstrapModal = bootstrap.Modal.getInstance(modal);
  bootstrapModal.hide();

  let id = numsele.value

  for (let i = 0; i < registros.value.length; i++) {


    if (registros.value[i].boleta === id) {

      registros.value[i].estadoTexto = "Pagado"


    }
  }
  totalDinero()
  descontarD()
}

function ganador() {


  Swal.fire({
    icon: "success",
    title: "Boleta Ganadora",
    showConfirmButton: false,
    timer: 2500
  })

  let modal = document.getElementById('exampleModal');
  let bootstrapModal = bootstrap.Modal.getInstance(modal);
  bootstrapModal.hide();

  let validar = false;

  let id = numsele.value
  let ganador = 0;

  registros.value.forEach(e => {
    console.log(e);
    if (e.estadoTexto === "Ganador") {
      ganador++
    }
  });

  for (let i = 0; i < registros.value.length; i++) {



    if (ganador > 0) {

      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "No puede haber mas de un Ganador",
        timer: 3500
      });

      return;
    } else {
      if (registros.value[i].boleta === id) {

        registros.value[i].estadoTexto = "Ganador"
        validar = true;

      }
    }
  }
  console.log(validar);
  if (validar === true) {
    arr.value[i.value].estado = 3
    validar = false;
  }
}



function validar() {
  let fecha_actual = new Date()
  let fecha_select = new Date(fecha.value);
  
  
  if (edit == true) {
    if (vrifa.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede estar vacio",
        timer: 3500
      });
    } else if (vrifa.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede menor o igual a 0",
        timer: 3500
      });
    } else if (isNaN(vrifa.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio debe ser numerico",
        timer: 3500
      });
    } else if (vboleta.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede estar vacio",
        timer: 3500
      });
    } else if (vboleta.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede ser menor o igual a 0",
        timer: 3500
      });
    }
    else if (isNaN(vboleta.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta debe ser numerico",
        timer: 3500
      });
    } else if (parseInt(vboleta.value) > parseInt(vrifa.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede ser superior al premio",
        timer: 3500
      });
    }
    else if (loterias.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la loteria que quieras jugar",
        timer: 3500
      });
    } else if (cantboletas.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la cantidad de boletas",
        timer: 3500
      });
    } else if (fecha.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la fecha del sorteo",
        timer: 3500
      });
    } else if (fecha_actual.setHours(0, 0, 0, 0) >= fecha_select) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "La fecha seleccionada no puede ser anterior o igual a la fecha actual, Por favor seleccione una fecha posterior",
        timer: 5500
      });
    } else {
      Swal.fire({
        icon: "success",
        title: "Datos de loteria registrados",
        showConfirmButton: false,
        timer: 3500
      });

      agregar()
      modal_intro.value = false;

    }
  } else {
    if (vrifa.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede estar vacio",
        timer: 3500
      });
    } else if (vrifa.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede menor o igual a 0",
        timer: 3500
      });
    } else if (isNaN(vrifa.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio debe ser numerico",
        timer: 3500
      });
    } else if (vboleta.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede estar vacio",
        timer: 3500
      });
    } else if (vboleta.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede ser menor o igual a 0",
        timer: 3500
      });
    } else if (isNaN(vboleta.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta debe ser numerico",
        timer: 3500
      });
    } else if (parseInt(vboleta.value) > parseInt(vrifa.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede ser superior al premio",
        timer: 3500
      });
    }
    else if (loterias.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la loteria que quieras jugar",
        timer: 3500
      });
    }
    else if (fecha.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la fecha del sorteo",
        timer: 3500
      });
    } else if (fecha_actual.setHours(0, 0, 0, 0) > fecha_select) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "La fecha seleccionada no puede ser anterior o igual a la fecha actual, Por favor seleccione una fecha posterior",
        timer: 5500
      });

    } else {
      datostalonario.value[index].vrifa = parseFloat(vrifa.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' });
      datostalonario.value[index].vboleta = parseFloat(vboleta.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' });
      datostalonario.value[index].loterias = loterias.value;
      datostalonario.value[index].cantboletas = cantboletas.value;
      datostalonario.value[index].fecha = fecha.value;
      edit = true;
      limpiar();
      
      cerrar3();
      Swal.fire({
        icon: "success",
        title: "Datos de loteria editados",
        showConfirmButton: false,
        timer: 3500
      });
    }

    
  }

}


function agregar() {

  const talonario = {
    vrifa: parseFloat(vrifa.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' }),
    vboleta: parseFloat(vboleta.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' }),
    loterias: loterias.value,
    cantboletas: parseInt(cantboletas.value),
    fecha: fecha.value,
  };


  datostalonario.value.push(talonario);


  arr.value = Array.from({ length: cantboletas.value }, (value, index) => ({
    numero: index,
    estado: 0,
    comprador: {}
  }));

  console.log(arr.value);
  console.log(datostalonario.value);
  limpiar();

}


function editar(item, i) {
  console.log(item);
  console.log(i);
  let numero = revertirFormatoMoneda(item.vboleta);
  let num = revertirFormatoMoneda(item.vrifa);

  vrifa.value = num;
  vboleta.value = numero;
  loterias.value = item.loterias;
  fecha.value = item.fecha;
  cantboletas.value = item.cantboletas;
  selectDeshabilitado.value = true;
  edit = false;
  index = i;
  modal_intro.value = true;

}

function limpiar() {
  vrifa.value = "";
  vboleta.value = "";
  loterias.value = "";
  cantboletas.value = "";
  fecha.value = "";

}

function limpiar2() {
  nombreC.value = "";
  direccionC.value = "";
  telefonoC.value = "";
  estadoC.value = "";

}

function revertirFormatoMoneda(valorMoneda) {

  let numero = valorMoneda.replace(/[^0-9,-]+/g, "");

  numero = numero.replace(',', '.');

  return parseInt(numero);
}

function totalDinero() {
  let numero = revertirFormatoMoneda(datostalonario.value[0].vboleta);

  let existe = false

  registros.value.forEach(e => {
    if (e.estadoTexto === "Pagado") {
      existe = true
      con.value++

    }
  });

  if (existe === true) {
    acum.value = numero * con.value
  }
  con.value = 0

  vpagada.value = parseFloat(acum.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' });
}

function descontar() {
  let numero = revertirFormatoMoneda(datostalonario.value[0].vboleta);

  acum.value -= numero

  vpagada.value = parseFloat(acum.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' });
}

function totalDeuda() {
  let numero = revertirFormatoMoneda(datostalonario.value[0].vboleta);

  let existe = false

  registros.value.forEach(e => {
    if (e.estadoTexto === "Apartado") {
      existe = true
      conn.value++

    }
  });

  if (existe === true) {
    acumm.value = numero * conn.value
  }
  conn.value = 0

  vdeuda.value = parseFloat(acumm.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' });

}

function descontarD() {
  let numero = revertirFormatoMoneda(datostalonario.value[0].vboleta);

  acumm.value -= numero

  vdeuda.value = parseFloat(acumm.value).toLocaleString('es-CO', { style: 'currency', currency: 'COP' });
}


function editarParticipante() {
  let texto = /^[A-Za-zÁÉÍÓÚáéíóúñÑüÜ\s]+$/;


  if(nombreP.value ==""){
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El nombre del comprador es requerido",
      timer: 3500
    });
    return;
  }else if (!texto.test(nombreP.value)) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de nombre comprador no puede llevar numeros",
      timer: 3500
    });
    return
  } else if(direccionP.value ==""){
 Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "La dirrecion del comprador es requerida",
      timer: 3500
    });
    return;
  }else if(telefonoP.value ==""){
  Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El telefono del comprador es requerido",
        timer: 3500
      });
      return;
  } else if (isNaN(telefonoP.value) == true) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono del comprador debe ser numerico",
      timer: 3500
    });
    return;
  } else if (telefonoP.value.length != 10) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono debe tener al menos 10 numeros",
      timer: 3500
    });
    return;
  } 

registros.value.forEach(e => {
  if(e.boleta === numsele.value){
    estadoP.value = e.estado
    estadoTextoP.value = e.estadoTexto
  }
});

  const cliente = {
    nombre: nombreP.value,
    direccion: direccionP.value,
    telefono: telefonoP.value,
    fecha: new Date().toISOString().split('T')[0],
    estado: estadoP.value,
    estadoTexto: estadoTextoP.value,
    boleta: numsele.value
  }

  arr.value[i.value].comprador = cliente

  console.log(registros.value);


  for (let i = 0; i < registros.value.length; i++) {
    const e = registros.value[i];
    
    if(e.boleta === numsele.value){
      registros.value[i]=cliente
    }
  }
  console.log(registros.value);


  let modal = document.getElementById('participanteEdit');
  let bootstrapModal = bootstrap.Modal.getInstance(modal);
  bootstrapModal.hide();

  Swal.fire({
      icon: "success",
      title: "Boleta Actualizada",
      showConfirmButton: false,
      timer: 1500
    });
}
</script>