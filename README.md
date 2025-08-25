using System.Collections.Generic;

namespace Smart_Frige1
{
    internal class Program
    {
        public class Food
        {
            public string name {  get; set; }
            public int amount_in_frige {  get; set; }
            public DateTime experationDate {  get; set; }
            public string commentAboutFood {  get; set; }
            public string reminderRegardingFood {  get; set; }

            public Food(string name, int amount_in_frige, DateTime experationDate)
            {
                this.name = name;
                this.amount_in_frige = amount_in_frige;
                this.experationDate = experationDate;
                this.commentAboutFood = "";
                this.reminderRegardingFood = "";
            }

            public override bool Equals(object? obj)
            {
                return base.Equals(obj);
            }

            public override int GetHashCode()
            {
                return base.GetHashCode();
            }

            public override string? ToString()
            {
                return base.ToString();
            }
        }
        public class Drink
        {
            public string name {  get; set; }
            public int amount_in_frige {  get; set; }
            public DateTime experationDate {  get; set; }
            public string commentAboutDrink { get; set; }
            public string reminderRegardingDrink { get; set; }

            public Drink(string name, int amount_in_frige, DateTime experationDate)
            {
                this.name = name;
                this.amount_in_frige = amount_in_frige;
                this.experationDate = experationDate;
                this.commentAboutDrink = "";
                this.reminderRegardingDrink = "";
            }
            public override bool Equals(object? obj)
            {
                return base.Equals(obj);
            }

            public override int GetHashCode()
            {
                return base.GetHashCode();
            }

            public override string? ToString()
            {
                return base.ToString();
            }
        }
        public class ExpirationManager
        {
            public Dictionary<string, DateTime> expirationDateDictionary {  get; set; }

            public ExpirationManager(List<Food> foodsInFrige, List<Drink> drinksInFrige)
            {
                expirationDateDictionary = new Dictionary<string, DateTime>();
                foreach (Food food in foodsInFrige)
                {
                    expirationDateDictionary.Add(food.name, food.experationDate);
                }
                foreach (Drink drink in drinksInFrige)
                {
                    expirationDateDictionary.Add(drink.name, drink.experationDate);
                }
            }
            public void AddFoodToDictionary(Food food)
            {
                if (food != null && !(expirationDateDictionary.ContainsKey(food.name)))
                {
                    expirationDateDictionary.Add(food.name, food.experationDate);
                }
            }
            public void AddDrinkToDictionary(Drink drink)
            {
                if (drink != null && !(expirationDateDictionary.ContainsKey(drink.name)))
                {
                    expirationDateDictionary.Add(drink.name, drink.experationDate);
                }
            }
            public void RemoveFoodFromDictionary(Food food)
            {
                if (food != null && expirationDateDictionary.ContainsKey(food.name))
                {
                    expirationDateDictionary.Remove(food.name);
                }
            }
            public void RemoveDrinkFromDictionary(Drink drink)
            {
                if (drink != null && expirationDateDictionary.ContainsKey(drink.name))
                {
                    expirationDateDictionary.Remove(drink.name);
                }
            }
            public override bool Equals(object? obj)
            {
                return base.Equals(obj);
            }

            public override int GetHashCode()
            {
                return base.GetHashCode();
            }

            public override string? ToString()
            {
                return base.ToString();
            }
        }
        public class UserProfile
        {
            public List<string> userPreferences {  get; set; }
            public List<string> itmesUserCannotEat {  get; set; }
            public List<string> favoriteItemsOfUser { get; set; }

            public UserProfile(List<string> userPreferences, List<string> itmesUserCannotEat, List<string> favoriteItemsOfUser)
            {
                this.userPreferences = userPreferences;
                this.itmesUserCannotEat = itmesUserCannotEat;
                this.favoriteItemsOfUser = favoriteItemsOfUser;
            }

            public override bool Equals(object? obj)
            {
                return base.Equals(obj);
            }

            public override int GetHashCode()
            {
                return base.GetHashCode();
            }

            public override string? ToString()
            {
                return base.ToString();
            }
        }
        public class Smart_Refrigerator
        {
            public List<Food> foodsInFrige {  get; set; }
            public List<Drink> drinksInFrige { get; set; }
            public Dictionary<string, float> itemPrices {  get; set; }
            public List<Tuple<string, int>> itemsNeededInFrige {  get; set; }
            public List<Tuple<string, int>> shoppingList {  get; set; }
            public float shoppingListPrice {  get; set; }
            ExpirationManager expirationManager { get; set; }


            public Smart_Refrigerator(List<Tuple<string, int>> usersRequestForFoodInFrige)
            {
                this.foodsInFrige = new List<Food>();
                this.drinksInFrige = new List<Drink>();
                this.itemPrices = new Dictionary<string, float>();
                this.itemsNeededInFrige = usersRequestForFoodInFrige;
                this.shoppingList = new List<Tuple<string, int>>();
                shoppingListPrice = 0;
                expirationManager = new ExpirationManager(this.foodsInFrige, this.drinksInFrige);
            }
            public void Get_Alert_Of_Expiration()
            {
                foreach(KeyValuePair<string, DateTime> keyValuePair in this.expirationManager.expirationDateDictionary)
                {
                    if (keyValuePair.Value <= DateTime.Now.AddDays(3))
                    {
                        Console.WriteLine("The {0} is about to expire!!!", keyValuePair.Key);
                    }
                }
            }
            public List<Tuple<string, int>> Create_Shopping_List()
            {
                List<Tuple<string, int>> shopList = new List<Tuple<string, int>>();
                foreach (Tuple<string, int> itemSet in this.itemsNeededInFrige)
                {
                    foreach (Food food in this.foodsInFrige)
                    {
                        if(food.name == itemSet.Item1)
                        {
                            if(food.amount_in_frige < itemSet.Item2)
                            {
                                shopList.Add(new Tuple<string, int>(food.name, itemSet.Item2 - food.amount_in_frige));
                                shoppingListPrice += (itemSet.Item2 - food.amount_in_frige) * itemPrices[food.name];
                            }
                        }
                    }
                    foreach (Drink drink in this.drinksInFrige)
                    {
                        if (drink.name == itemSet.Item1)
                        {
                            if (drink.amount_in_frige < itemSet.Item2)
                            {
                                shopList.Add(new Tuple<string, int>(drink.name, itemSet.Item2 - drink.amount_in_frige));
                                shoppingListPrice += (itemSet.Item2 - drink.amount_in_frige) * itemPrices[drink.name];
                            }
                        }
                    }
                }
                return shopList;
            }
            public void UpdatePriceDictionary(string item, float price)
            {
                if (itemPrices.ContainsKey(item))
                {
                    itemPrices[item] = price;
                }
                else
                {
                    itemPrices.Add(item, price);
                }
            }
            public void Add_Food(string name, int amount, DateTime expDate)
            {
                Food newFood = new Food(name, amount, expDate);
                foodsInFrige.Add(newFood);
            }
            public void Add_Drink(string name, int amount, DateTime expDate)
            {
                Drink newDrink = new Drink(name, amount, expDate);
                drinksInFrige.Add(newDrink);
            }

            public void Update_Food_Taken_Out(Food food, int amountTakenOut)
            {
                food.amount_in_frige -= amountTakenOut;
                if(food.amount_in_frige <= 0)
                {
                    foodsInFrige.Remove(food);
                }
            }
            public void Update_Drink_Taken_Out(Drink drink, int amountTakenOut)
            {
                drink.amount_in_frige -= amountTakenOut;
                if (drink.amount_in_frige <= 0)
                {
                    drinksInFrige.Remove(drink);
                }
            }
            public void Update_Food_Put_In(Food food, int amountPutIn)
            {
                if (foodsInFrige.Contains(food))
                {
                    food.amount_in_frige += amountPutIn;
                }
                else
                {
                    foodsInFrige.Add(food);
                    food.amount_in_frige = amountPutIn;

                }
            }
            public void Update_Drink_Put_In(Drink drink, int amountPutIn)
            {
                if (drinksInFrige.Contains(drink))
                {
                    drink.amount_in_frige += amountPutIn;
                }
                else
                {
                    drinksInFrige.Add(drink);
                    drink.amount_in_frige = amountPutIn;

                }
            }

            public override bool Equals(object? obj)
            {
                return base.Equals(obj);
            }

            public override int GetHashCode()
            {
                return base.GetHashCode();
            }

            public override string? ToString()
            {
                return base.ToString();
            }
        }
        public static string GetCommandFromUser()
        {
            Console.Write("Please choose a command! ");
            Console.WriteLine("(content of frige, exp date of products, shopping list, add comment, add reminder, exit)");
            string usersRequest = Console.ReadLine();
            return usersRequest;
        }
        static void Main(string[] args)
        {
            string usersRequest = GetCommandFromUser();
            while (true)
            {
                if(usersRequest.Equals("content of frige"))
                {

                }
                else if(usersRequest.Equals("exp date of products"))
                {

                }
                else if (usersRequest.Equals("shopping list"))
                {

                }
                else if (usersRequest.Equals("add comment"))
                {
                    Console.WriteLine("To what whould you like to add it?(Choose Food or Drink)");
                    string itemType = Console.ReadLine();
                }
                else if (usersRequest.Equals("add reminder"))
                {
                    Console.WriteLine("To what whould you like to add it?(Choose Food or Drink)");
                    string itemType = Console.ReadLine();
                }
                else if (usersRequest.Equals("exit"))
                {
                    break;
                }
                else
                {
                    Console.WriteLine("I did not understand the command. Please try again!");
                    usersRequest = GetCommandFromUser();

                }
            }
        }
    }
}
