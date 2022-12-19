You can use the fixture design pattern to facilitate the creation of mock data. With this pattern, you only need to change the parameters that you want to modify. Without it, you would have to declare the same variables repeatedly. By using default values for the parameters, you can just change the properties that you actually need to modify.

For example, you can use the fixture design pattern like this:
```swift
struct Movie: Decodable {
    let adult: Bool
    let backdropPath: String
    let genreIDS: [Int]
    let id: Int
    let originalLanguage: String
    let originalTitle: String
    let overview: String
    let popularity: Double
    let posterPath, releaseDate, title: String
    let video: Bool
    let voteAverage: Double
    let voteCount: Int
}

extension Movie {
   static func fixture(
        adult: Bool = false,
        backdropPath: String = "",
        genreIDS: [Int] = [],
        id: Int = "123",
        originalLanguage: String = "pt-BR",
        originalTitle: String = "Test"
    ) -> Self {
        return Movie.init(
            adult: adult,
            backdropPath: backdropPath,
            genreIDS: genreIDS,
            id: id,
            originalLanguage: originalLanguage,
            originalTitle: originalTitle
        )
    }
}
  
  // Usage: If you just want to change these two properties, you just need to pass the values that you want on the static func
  static var movie: Movie = .fixture(adult: true, originalLanguage: "en-US")
```

## References
- [Swift Tests Tips & Tricks: Fixture Object Pattern](https://medium.com/@bruno.hcr/swift-tests-tips-tricks-fixture-object-pattern-5decefe6f10c)
