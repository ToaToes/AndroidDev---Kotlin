## 1. Local Data persistence, SQLite by ROOM -> Thread safe

Using a singleton pattern for your Room database is a common and effective approach in Android development. This ensures that you have a single instance of the database throughout your app, which helps to avoid multiple connections and potential issues with data consistency.

1. Define Your Entity
First, create an entity that represents the data you want to store in your database.
```
@Entity(tableName = "products")
data class Product(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val name: String,
    val price: Double
)

```

2. Create a Data Access Object (DAO)
Define a DAO interface to specify the methods for accessing the database.
```
@Dao
interface ProductDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(product: Product)

    @Query("SELECT * FROM products")
    suspend fun getAllProducts(): List<Product>

    @Delete
    suspend fun delete(product: Product)
}

```

3. Create the Room Database
Define the Room database class and use the singleton pattern to ensure only one instance is created.
```
@Database(entities = [Product::class], version = 1, exportSchema = false)
abstract class AppDatabase : RoomDatabase() {
    abstract fun productDao(): ProductDao

    companion object {
        @Volatile
        private var INSTANCE: AppDatabase? = null

        fun getDatabase(context: Context): AppDatabase {
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    AppDatabase::class.java,
                    "app_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}

```

4. Use the Database in Your Repository
You can create a repository class to interact with the Room database.
```
class ProductRepository(private val productDao: ProductDao) {

    suspend fun insertProduct(product: Product) {
        productDao.insert(product)
    }

    suspend fun getAllProducts(): List<Product> {
        return productDao.getAllProducts()
    }
}

```

5. Access the Database from Your Activities or Fragments
Now, you can access the database from any activity or fragment using the repository.
```
class ProductListActivity : AppCompatActivity() {

    private lateinit var productRepository: ProductRepository

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_product_list)

        val database = AppDatabase.getDatabase(this)
        productRepository = ProductRepository(database.productDao())

        // Example: Insert a product in the database
        lifecycleScope.launch {
            val newProduct = Product(name = "Example Product", price = 9.99)
            productRepository.insertProduct(newProduct)

            // Fetch all products and display them
            val products = productRepository.getAllProducts()
            println(products)
        }
    }
}

```
