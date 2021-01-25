console.log("ZEPEDA ARANDA LUIS ALBERTO")

 // Your web app's Firebase configuration
var firebaseConfig = {
    apiKey: "AIzaSyCEkzG89YKPvRCoxTCAsRDpOKbUjs-97xI",
    authDomain: "webproyecto-84b3a.firebaseapp.com",
    projectId: "webproyecto-84b3a",
    storageBucket: "webproyecto-84b3a.appspot.com",
    messagingSenderId: "1064575792334",
    appId: "1:1064575792334:web:ec8fee2f98a327153d0c9f"
  };
  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);

  var db = firebase.firestore();

let arregloLibros = [];

  db.collection("libros").get().then((querySnapshot) => {
    querySnapshot.forEach((doc) => {

    	var obj = doc.data()
        obj.id = doc.id;
        arregloLibros.push(obj);
    });
});
  
  console.log(arregloLibros)

  function leerDatosLibro(){
  	var id = document.getElementById("libroaEliminar").value

  	var docRef = db.collection("libros").doc(id);

docRef.get().then(function(doc) {
    if (doc.exists) {
        console.log("Document data:", doc.data());
        var libroSeleccionado = doc.data();
        console.log(libroSeleccionado.Nombre)

          	document.getElementById("libroAutor").value =libroSeleccionado.Autor
  	document.getElementById("nombreNuevoLibro").value=libroSeleccionado.Nombre
  	document.getElementById("libroEditorial").value=libroSeleccionado.Editorial

    } else {
        // doc.data() will be undefined in this case
        console.log("No such document!");
    }
}).catch(function(error) {
    console.log("Error getting document:", error);
});



  }






  function agregarDatos(){
  	var autor = document.getElementById("libroAutor").value
  	var nombre = document.getElementById("nombreNuevoLibro").value
  	var password= document.getElementById("libroEditorial").value

  	if(autor == "" || autor == null){
  		alert("El autor no puede estar vacio")
  	}else{
  		  db.collection("libros").add({

    Autor: autor,
    Editorial: password,
    Nombre: nombre
})
.then(function(docRef) {
    alert("El libro se agrego correctamente")
})
.catch(function(error) {
    console.error("Error adding document: ", error);
});

  	}

  	

  }

function borrarDatosLibros(){
	console.log("Se ejecuto la funcion eliminar datos")
	var id = document.getElementById("libroaEliminar").value

	for(var i = 0 ; i < arregloLibros.length;i++){
if(arregloLibros[i].Autor == id){
	console.log("El id del libro que quieres eliminar es : " +arregloLibros[i].id )

}
		

	}
	/*
	db.collection("libros").doc(id).delete().then(function() {
    alert("El libro se elimino correctamente")
}).catch(function(error) {
    console.error("Error removing document: ", error);
});*/
}

function editarLibro(){



	var id = document.getElementById("libroaEliminar").value


	for(var i = 0 ; i < arregloLibros.length;i++){
if(arregloLibros[i].Autor == id){
	console.log("El id del libro que quieres editar es : " +arregloLibros[i].id )

}
}

	var autor = document.getElementById("libroAutor").value
  	var nombre = document.getElementById("nombreNuevoLibro").value
  	var password= document.getElementById("libroEditorial").value
	var libroAeditar = db.collection("libros").doc(id);

// Set the "capital" field of the city 'DC'
return libroAeditar.update({
    "Nombre": nombre,
    "Editorial":password,
    "Autor":autor
})
.then(function() {
    console.log("Document successfully updated!");
})
.catch(function(error) {
    // The document probably doesn't exist.
    console.error("Error updating document: ", error);
});

}