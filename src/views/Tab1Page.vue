<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title class="ion-text-center">Verdulería Carlitos</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" class="ion-padding">
      <ion-grid>
        <ion-card color="success" class="card" v-for="p in productos" :key="p.id">
          <ion-card-content class="card-content">
            <ion-row>
              <ion-col size="2" class="colimagen">
                <img :src = "p.imagen" class="imgproducto" />
              </ion-col>
              <ion-col size="10" class="coldatos, ion-padding">
                <ion-card-subtitle>
                  <ion-row>
                    <ion-col size="auto">
                      <ion-badge>
                        {{ p.categoria_nombre }}
                      </ion-badge>
                    </ion-col>

                    <ion-col  class="ion-text-end">
                      <ion-button  size="small">
                        <ion-icon :icon="cart"></ion-icon>
                      </ion-button>
                    </ion-col>

                  </ion-row>

                </ion-card-subtitle>
                

                <ion-card-title>
                  {{ p.nombre }}
                </ion-card-title>
                
                <ion-card-subtitle>
                  ${{ p.precio }}   -   Precio:{{ p.precio_tipo }}
                </ion-card-subtitle>
                <ion-card-subtitle>
                  {{ p.descripcion }}
                </ion-card-subtitle>
              </ion-col>

            </ion-row>
            
          </ion-card-content>
        </ion-card>
      </ion-grid>

      <ion-card v-for="p in productos" :key="p.id">
        <img
          :src= "p.imagen"  
          
        />
        <ion-card-header>
          <ion-card-title class="ion-text-center"> {{ p.nombre }} </ion-card-title>
          
        </ion-card-header>
    
        <ion-card-content>
          {{ p.descripcion }}
        </ion-card-content>
        <hr>
      </ion-card>
      
    </ion-content>
  </ion-page>
</template>


<script setup lang="ts">

import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonCard, IonCardContent, IonCardHeader,  IonCardTitle, IonGrid, IonCol, IonCardSubtitle, IonBadge, IonIcon, IonRow, IonButton } from '@ionic/vue';

import {cart} from 'ionicons/icons';

import { ref, onMounted } from 'vue';

interface IPosts {
    id: number;
    nombre: string;
    imagen: string;
    descripcion: string;
    precio: number;
    precio_tipo: string;
    categoria_nombre: string;
}

const error = ref('')
const productos = ref<IPosts[]>([])


const getData = async ():Promise<void> =>  {

  const url = 'https://panambi.pythonanywhere.com/api/productos/'

/*   const response = await fetch (url)

  if (!response.ok) {
    throw new error('No se pudo obtener la información solicitada!!')
  }
  productos.value = await response.json() */

  try {
    const response = await fetch(url);
    if (!response.ok) {
        const errorDetails = await response.text();
        throw new Error(`Error ${response.status}: ${errorDetails}`);
    }
    productos.value = await response.json();
  } catch (err) { // Cambia el nombre a err para evitar confusiones con el objeto global Error
    console.error('Error fetching data:', err);
    error.value = (err as Error).message; // Manejo correcto del tipo de error
  }


}

onMounted (()=>{
  getData()
})

</script>

<style scoped>

.card {
  border-radius: 10px;
  margin: 20px 0px;
}

.card-content {
  display: contents;
}
.colimagen {
  justify-content: center;
  align-self: center;;
}

.coldatos {
  justify-content: center;
  margin: auto;
  padding-left: 5px;
  
}

.imgproducto {
  position: relative;
  height: 50px;
  
  max-width: 50px;
  border-radius: 10px;
}

</style>