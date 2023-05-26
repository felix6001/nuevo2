# nuevo2
data class Mascota(
    val nombre: String,
    val raza: String,
    val edad: Int,
    val genero: String,
    val foto: String,
    val descripcion: String,
    var raiting: Int
)

class MascotaAdapter(private val listaMascotas: List<Mascota>) : RecyclerView.Adapter<MascotaAdapter.MascotaViewHolder>() {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MascotaViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_mascota, parent, false)
        return MascotaViewHolder(view)
    }

    override fun onBindViewHolder(holder: MascotaViewHolder, position: Int) {
        val mascota = listaMascotas[position]
        holder.bind(mascota)
    }

    override fun getItemCount(): Int {
        return listaMascotas.size
    }

    inner class MascotaViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        private val imagenMascota: ImageView = itemView.findViewById(R.id.imagenMascota)
        private val nombreMascota: TextView = itemView.findViewById(R.id.nombreMascota)
        private val iconoHueso: ImageView = itemView.findViewById(R.id.iconoHueso)
        private val raitingMascota: TextView = itemView.findViewById(R.id.raitingMascota)

        fun bind(mascota: Mascota) {
            nombreMascota.text = mascota.nombre
            // Cargar la imagen de la mascota

            // Verificar el raiting de la mascota y configurar el icono de hueso adecuado
            if (mascota.raiting > 0) {
                iconoHueso.setImageResource(R.drawable.hueso_amarillo)
            } else {
                iconoHueso.setImageResource(R.drawable.hueso_blanco)
            }
            
            // Configurar el evento de click en el ícono de hueso para raitear la mascota
            iconoHueso.setOnClickListener {
                mascota.raiting++
                notifyDataSetChanged()
            }
        }
    }
}
  
class FavoriteMascotasActivity : AppCompatActivity() {
    private lateinit var recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_favorite_mascotas)

        recyclerView = findViewById(R.id.recyclerViewFavoriteMascotas)
        recyclerView.layoutManager = LinearLayoutManager(this)

        val mascotasDummy = getDummyMascotas() // Obtener una lista de mascotas dummy hardcodeadas
        val adapter = MascotaAdapter(mascotasDummy)
        recyclerView.adapter = adapter
    }

    private fun getDummyMascotas(): List<Mascota> {
        // Crear una lista de mascotas dummy hardcodeadas (5 mascotas)
        val mascotas = mutableListOf<Mascota>()
               mascotas.add(Mascota("Mascota 1", "Raza 1", 3, "Macho", "foto1.jpg", "Descripción 1", 0))
        mascotas.add(Mascota("Mascota 2", "Raza 2", 2, "Hembra", "foto2.jpg", "Descripción 2", 0))
        mascotas.add(Mascota("Mascota 3", "Raza 3", 4, "Macho", "foto3.jpg", "Descripción 3", 0))
        mascotas.add(Mascota("Mascota 4", "Raza 4", 1, "Hembra", "foto4.jpg", "Descripción 4", 0))
        mascotas.add(Mascota("Mascota 5", "Raza 5", 5, "Macho", "foto5.jpg", "Descripción 5", 0))
        return mascotas
    }
}
    class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Configurar el botón de acción en la barra de herramientas
        val actionButton: ImageButton = findViewById(R.id.actionButton)
        actionButton.setOnClickListener {
            val intent = Intent(this, FavoriteMascotasActivity::class.java)
            startActivity(intent)
        }
    }
}

    
