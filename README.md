### Dataset

https://www.kaggle.com/datasets/netflix-inc/netflix-prize-data

### CreateML

1. Create Recommendation
2. Prepare CSV File (Two Colums, one for user, one for item)
3. Create mlmodel
   - User => Group
   - Item => Actual Item

### Code

```swift

import SwiftUI

struct ContentView: View {

    let model = RecipeRecommender()

    @State private var recommendations: [String] = [String]()
    @State private var incredient: String = ""
    @State private var incredients: [String] = [String]()

    private func performRecommendations() {
        let tuples = self.incredients.map { ingredient in (ingredient, 5.0) }
        let items = Dictionary(uniqueKeysWithValues: tuples)
        guard let output = try? self.model.prediction(items: items, k: 5, restrict_: [], exclude: self.incredients) else { return }
        self.recommendations = output.recommendations
    }

    var body: some View {

        NavigationView {

            VStack(alignment: .leading) {
                HStack {
                    TextField("Incredient", text: $incredient)
                        .textFieldStyle(RoundedBorderTextFieldStyle())
                    Button("Add") {
                        self.incredients.append(self.incredient)
                        self.incredient = ""
                        // perform recommendations
                        self.performRecommendations()
                    }
                    .padding(10)
                    .foregroundColor(Color.white)
                    .background(Color.green)
                    .cornerRadius(6)

                }.padding(10)

                IncredientsView(incredients: self.incredients)`
                Text("Recommendations")
                    .padding(10)
                    .font(.title)

                RecommendationsView(recommendations: self.recommendations, incredientAdded: { ingredient in
                    self.incredients.append(ingredient)
                    self.performRecommendations()
                })

                Spacer()

            }

            .navigationBarTitle("Shopping List")
        }

    }
}

```
