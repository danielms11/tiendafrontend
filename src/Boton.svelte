<script>
    import { onMount } from "svelte";

    export let tipo = "insertar";
    export let documento = {};

    import {data} from "./store"
    import { getContext } from "svelte";
    let URL = getContext("URL");

    let handler = () => {};

    function insertar() {
        let opciones = {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(documento),
        };
        fetch(URL.articulos, opciones)
            .then((res) => res.json())
            .then(doc => $data = [...$data, doc])
            .catch((error) => console.log(error));
    }

    function modificar() {
        console.log("modificar");
        let opciones = {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(documento),
        };
        fetch(URL.articulos + documento._id, opciones)
            .then((res) => res.json())
            .then((datos) => console.log(datos))
            .catch((error) => console.log(error));
    }

    function eliminar() {
        let opciones = {
            method: "DELETE",
        };
        fetch(URL.articulos + documento._id, opciones)
            .then((res) => res.json())
            .then($data =$data.filter (doc => doc._id != documento._id))
            .catch((error) => console.log(error));
    }

    function setup() {
        switch (tipo) {
            case "insertar":
                handler = insertar;
                break;
            case "modificar":
                handler = modificar;
                break;
            case "eliminar":
                handler = eliminar;
                break;
            default:
        }
    }

    onMount(setup);
</script>

<input type="button" value={tipo.toUpperCase()} on:click={handler} />
