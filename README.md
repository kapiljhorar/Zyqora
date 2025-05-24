# Zyqora
"use client"

import { useState, useEffect } from "react"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import {
  Loader2,
  RefreshCw,
  AlertCircle,
  Star,
  ShoppingCart,
} from "lucide-react"

interface Product {
  id: string
  title: string
  description: string
  price: number
  rating: number
  category: string
  imageUrl: string
}

export default function ProductRecommendations() {
  const [products, setProducts] = useState<Product[]>([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<string | null>(null)
  const [cartItems, setCartItems] = useState<Product[]>([])

  // Dummy products with images (replace images with your own URLs)
  const dummyProducts: Product[] = [
    {
      id: "1",
      title: "Artisan Coffee Blend",
      description:
        "Premium single-origin coffee beans with notes of chocolate and caramel",
      price: 24.99,
      rating: 4.8,
      category: "Coffee",
      imageUrl:
        "https://images.unsplash.com/photo-1509042239860-f550ce710b93?auto=format&fit=crop&w=400&q=80",
    },
    {
      id: "2",
      title: "Handcrafted Ceramic Mug",
      description: "Unique pottery mug perfect for your morning coffee ritual",
      price: 18.5,
      rating: 4.6,
      category: "Accessories",
      imageUrl:
        "https://images.unsplash.com/photo-1525673475629-7a223eb5f2c7?auto=format&fit=crop&w=400&q=80",
    },
    {
      id: "3",
      title: "Organic Tea Collection",
      description:
        "Curated selection of premium organic teas from around the world",
      price: 32.0,
      rating: 4.9,
      category: "Tea",
      imageUrl:
        "https://images.unsplash.com/photo-1508385082359-fc7e56198a3d?auto=format&fit=crop&w=400&q=80",
    },
    {
      id: "4",
      title: "French Press Deluxe",
      description: "Professional-grade French press for the perfect brew every time",
      price: 45.99,
      rating: 4.7,
      category: "Equipment",
      imageUrl:
        "https://images.unsplash.com/photo-1519681393784-d120267933ba?auto=format&fit=crop&w=400&q=80",
    },
  ]

  const fetchRecommendations = async () => {
    setLoading(true)
    setError(null)
    try {
      // Replace this with your actual API call
      await new Promise((r) => setTimeout(r, 1500))
      setProducts(dummyProducts)
    } catch (err) {
      setError(err instanceof Error ? err.message : "Failed to load recommendations")
    } finally {
      setLoading(false)
    }
  }

  useEffect(() => {
    fetchRecommendations()
  }, [])

  const handleRetry = () => {
    fetchRecommendations()
  }

  const handleAddToCart = (product: Product) => {
    setCartItems((prev) => [...prev, product])
    alert(`Added "${product.title}" to cart!`)
  }

  return (
    <section className="py-16 sm:py-24">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        {/* Header */}
        <div className="text-center mb-12">
          <h2 className="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            AI-Powered Recommendations
          </h2>
          <p className="mt-4 text-lg text-gray-600">
            Personalized product suggestions tailored just for you
          </p>
        </div>

        {/* Loading */}
        {loading && (
          <div className="flex flex-col items-center justify-center py-12">
            <Loader2 className="h-8 w-8 animate-spin text-blue-600 mb-4" />
            <p className="text-gray-600">Loading personalized recommendations...</p>
          </div>
        )}

        {/* Error */}
        {error && (
          <div className="flex flex-col items-center justify-center py-12">
            <div className="rounded-lg bg-red-50 p-6 text-center max-w-md">
              <AlertCircle className="h-8 w-8 text-red-600 mx-auto mb-4" />
              <h3 className="text-lg font-semibold text-red-800 mb-2">
                Failed to Load Recommendations
              </h3>
              <p className="text-red-600 mb-4">{error}</p>
              <Button
                onClick={handleRetry}
                variant="outline"
                className="border-red-300 text-red-700 hover:bg-red-50"
                aria-label="Retry fetching recommendations"
              >
                <RefreshCw className="h-4 w-4 mr-2" />
                Try Again
              </Button>
            </div>
          </div>
        )}

        {/* Products Grid */}
        {!loading && !error && products.length > 0 && (
          <div className="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-4">
            {products.map((product) => (
              <Card
                key={product.id}
                className="group hover:shadow-lg transition-all duration-300 hover:-translate-y-1"
              >
                <CardHeader className="pb-4">
                  <div className="aspect-square rounded-lg mb-4 overflow-hidden">
                    <img
                      src={product.imageUrl}
                      alt={product.title}
                      className="object-cover w-full h-full"
                      loading="lazy"
                    />
                  </div>
                  <CardTitle className="text-lg font-semibold line-clamp-2">
                    {product.title}
                  </CardTitle>
                  <CardDescription className="line-clamp-2">
                    {product.description}
                  </CardDescription>
                </CardHeader>
                <CardContent className="pt-0">
                  <div className="flex items-center justify-between mb-4">
                    <span className="text-2xl font-bold text-gray-900">
                      ${product.price.toFixed(2)}
                    </span>
                    <div className="flex items-center space-x-1">
                      <Star className="h-4 w-4 fill-yellow-400 text-yellow-400" />
                      <span className="text-sm text-gray-600">{product.rating}</span>
                    </div>
                  </div>
                  <Button
                    className="w-full bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700"
                    onClick={() => handleAddToCart(product)}
                    aria-label={`Add ${product.title} to cart`}
                  >
                    Add to Cart
                  </Button>
                </CardContent>
              </Card>
            ))}
          </div>
        )}

        {/* No Products Message */}
        {!loading && !error && products.length === 0 && (
          <div className="text-center py-12">
            <p className="text-gray-600">No recommendations available at the moment.</p>
          </div>
        )}
      </div>
    </section>
  )
}

