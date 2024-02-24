# Ejercicio-3.-Manejo-de-Card
Ejercicio 3. Manejo de Card

//Main Activity
package com.mexiti.listacomidaor

import android.annotation.SuppressLint
import android.os.Bundle
import android.view.Menu
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.Image
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.material3.Card
import androidx.compose.material3.CenterAlignedTopAppBar
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.modifier.modifierLocalConsumer
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.mexiti.listacomidaor.data.DataSource
import com.mexiti.listacomidaor.model.Platillo
import com.mexiti.listacomidaor.ui.theme.ListaComidaOrTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            ListaComidaOrTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    MenuApp()
                }
            }
        }
    }
}

@Composable
fun MenuApp(){
    MenuCardList(platilloList = DataSource().LoadPlatillos()
        )
}
@Composable
fun MenuCardList( platilloList: List<Platillo>,modifier: Modifier= Modifier   ){
    Scaffold(
        topBar = {
            MenuTopAppBar()
        }
    ) {
        it ->
        LazyColumn(contentPadding = it){
            items( platilloList ){
                    platillo -> MenuCard(
                platillo = platillo,
                modifier = modifier.padding(10.dp)

            )
            }

        }
    }
}
@Composable
fun MenuCard(platillo:Platillo, modifier: Modifier = Modifier ){
    Card(
        modifier = modifier
    ){
        Row(
            modifier = modifier.fillMaxWidth()
        ){
            Image(
                painter = painterResource(id = platillo.drawableResourceId) ,
                contentDescription = stringResource(id = platillo.stringResourceId),
                modifier =
                modifier
                    .size(170.dp)
                    .clip(CircleShape),
                contentScale = ContentScale.Crop
            )
            Column {

                Text(
                    text = LocalContext.current.getString(platillo.stringResourceId),
                    modifier = modifier.padding(5.dp, 0.dp),
                    style = MaterialTheme.typography.displayMedium
                )
                Text(
                    text = LocalContext.current.getString(platillo.stringResourceId1),
                    modifier = modifier.padding(5.dp, 0.dp),
                    style = MaterialTheme.typography.displayMedium
                )
                Text(
                    text = LocalContext.current.getString(platillo.stringResourceId2),
                    modifier = modifier.padding(5.dp, 0.dp),
                    style = MaterialTheme.typography.displayMedium
                )
            }
        }

    }
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MenuTopAppBar(modifier: Modifier = Modifier){
    CenterAlignedTopAppBar(
        title = {
            Row(
                verticalAlignment = Alignment.CenterVertically
            ){
                Image(
                    painter = painterResource(id = R.drawable.restaurant),
                    contentDescription = null,
                    modifier = modifier
                        .padding(8.dp)
                        .size(64.dp)
                )
                Text(
                    text = stringResource(id = R.string.app_name),
                    style = MaterialTheme.typography.displayLarge
                )
            }
        },
        modifier = modifier

    )

}

@Composable
@Preview(showBackground = true)
fun ShowMenuCard(){
    ListaComidaOrTheme (darkTheme = true) {
        MenuCardList(platilloList = DataSource().LoadPlatillos()
        )
    }

}

//Colors.xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>

//Strings.xml
<resources>
    <string name="app_name">ListaComida</string>
    <string name="desayuno">Desayunos en familia</string>
    <string name="hamburger">Hamburguesa extra grande</string>
    <string name="pizza">Pizzas a la orden</string>
    <string name="postre">Disfruta de los postres </string>
    <string name="pozole">Cualquier día es noche mexicana con Pozole </string>
    <string name="tacos">Tacos de guisado </string>

    //Precios
    <string name="precioDesayuno">Desde $249 MX </string>
    <string name="preciohamburger">$85 MX </string>
    <string name="precioPizza">$299 MX</string>
    <string name="precioPostre">$60 MX</string>
    <string name="precioPozole">$115 MX</string>
    <string name="precioTacos">$22 MX</string>

    //Ofertas
    <string name="Ofertas">20% de descuento </string>

</resources>

//Data Platillo.kt
package com.mexiti.listacomidaor.model

import androidx.annotation.DrawableRes
import androidx.annotation.StringRes

data class Platillo(
    @StringRes val stringResourceId: Int,
    @DrawableRes val drawableResourceId: Int,
    @StringRes val stringResourceId1: Int,
    @StringRes val stringResourceId2: Int,

)


//DataSource.kt
package com.mexiti.listacomidaor.data

import com.mexiti.listacomidaor.R
import com.mexiti.listacomidaor.model.Platillo

class DataSource() {
    fun LoadPlatillos(): List<Platillo>{
        return listOf(
            Platillo(R.string.desayuno,R.drawable.desayuno,R.string.precioDesayuno, R.string.Ofertas),
            Platillo(R.string.hamburger,R.drawable.hamburger, R.string.preciohamburger,R.string.Ofertas),
            Platillo(R.string.pizza,R.drawable.pizza, R.string.precioPizza, R.string.Ofertas),
            Platillo(R.string.postre,R.drawable.postre, R.string.precioPostre,R.string.Ofertas),
            Platillo(R.string.pozole,R.drawable.pozole, R.string.precioPozole, R.string.Ofertas),
            Platillo(R.string.tacos,R.drawable.tacos, R.string.precioTacos,R.string.Ofertas)
        )
    }
}

//themes
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <style name="Theme.ListaComidaOr" parent="android:Theme.Material.Light.NoActionBar" />
</resources>
