using System;
using System.Collections.Generic;
using System.Linq;

namespace TipSharingPlatform
{
    // Tip class
    public class Tip
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
        public string Category { get; set; }
        public DateTime CreatedDate { get; set; }
        public int Upvotes { get; set; }
        public int Downvotes { get; set; }

        public Tip(string title, string description, string category)
        {
            Title = title;
            Description = description;
            Category = category;
            CreatedDate = DateTime.Now;
            Upvotes = 0;
            Downvotes = 0;
        }
    }

    // TipManager class
    public class TipManager
    {
        private List<Tip> _tips = new List<Tip>();
        private int _nextId = 1;

        public void AddTip(string title, string description, string category)
        {
            var tip = new Tip(title, description, category) { Id = _nextId };
            _tips.Add(tip);
            _nextId++;
        }

        public List<Tip> GetAllTips()
        {
            return _tips;
        }

        public Tip GetTipById(int id)
        {
            return _tips.FirstOrDefault(t => t.Id == id);
        }

        public void UpvoteTip(int id)
        {
            var tip = GetTipById(id);
            if (tip != null)
            {
                tip.Upvotes++;
            }
        }

        public void DownvoteTip(int id)
        {
            var tip = GetTipById(id);
            if (tip != null)
            {
                tip.Downvotes++;
            }
        }

        public List<Tip> GetTipsByCategory(string category)
        {
            return _tips.Where(t => t.Category.Equals(category, StringComparison.OrdinalIgnoreCase)).ToList();
        }

        public List<Tip> GetTopRatedTips(int count)
        {
            return _tips.OrderByDescending(t => t.Upvotes - t.Downvotes).Take(count).ToList();
        }
    }

    // Main program class
    public class Program
    {
        static void Main(string[] args)
        {
            // Create a TipManager instance
            var tipManager = new TipManager();

            // Add some sample tips
            tipManager.AddTip("Save Money on Groceries", "Make a meal plan and stick to it.", "Finance");
            tipManager.AddTip("Clean Your Keyboard", "Use a vacuum cleaner with a brush attachment.", "Tech");
            tipManager.AddTip("Make a DIY Bird Feeder", "Use a plastic bottle and some string.", "DIY");

            // Main program loop
            while (true)
            {
                Console.WriteLine("\nTip Sharing Platform");
                Console.WriteLine("1. Add Tip");
                Console.WriteLine("2. View All Tips");
                Console.WriteLine("3. View Tip by ID");
                Console.WriteLine("4. View Tips by Category");
                Console.WriteLine("5. View Top Rated Tips");
                Console.WriteLine("6. Upvote Tip");
                Console.WriteLine("7. Downvote Tip");
                Console.WriteLine("8. Exit");

                Console.Write("Enter your choice: ");
                int choice = int.Parse(Console.ReadLine());

                switch (choice)
                {
                    case 1:
                        Console.Write("Enter tip title: ");
                        string title = Console.ReadLine();
                        Console.Write("Enter tip description: ");
                        string description = Console.ReadLine();
                        Console.Write("Enter tip category: ");
                        string category = Console.ReadLine();
                        tipManager.AddTip(title, description, category);
                        Console.WriteLine("Tip added successfully.");
                        break;
                    case 2:
                        var allTips = tipManager.GetAllTips();
                        if (allTips.Any())
                        {
                            foreach (var tip in allTips)
                            {
                                Console.WriteLine($"\nID: {tip.Id}");
                                Console.WriteLine($"Title: {tip.Title}");
                                Console.WriteLine($"Description: {tip.Description}");
                                Console.WriteLine($"Category: {tip.Category}");
                                Console.WriteLine($"Created Date: {tip.CreatedDate:dd/MM/yyyy HH:mm:ss}");
                                Console.WriteLine($"Upvotes: {tip.Upvotes}");
                                Console.WriteLine($"Downvotes: {tip.Downvotes}");
                            }
                        }
                        else
                        {
                            Console.WriteLine("No tips found.");
                        }
                        break;
                    case 3:
                        Console.Write("Enter tip ID: ");
                        int id = int.Parse(Console.ReadLine());
                        var tip = tipManager.GetTipById(id);
                        if (tip != null)
                        {
                            Console.WriteLine($"\nID: {tip.Id}");
                            Console.WriteLine($"Title: {tip.Title}");
                            Console.WriteLine($"Description: {tip.Description}");
                            Console.WriteLine($"Category: {tip.Category}");
                            Console.WriteLine($"Created Date: {tip.CreatedDate:dd/MM/yyyy HH:mm:ss}");
                            Console.WriteLine($"Upvotes: {tip.Upvotes}");
                            Console.WriteLine($"Downvotes: {tip.Downvotes}");
                        }
                        else
                        {
                            Console.WriteLine("Tip not found.");
                        }
                        break;
                    case 4:
                        Console.Write("Enter tip category: ");
                        string categoryToSearch = Console.ReadLine();
                        var tipsByCategory = tipManager.GetTipsByCategory(categoryToSearch);
                        if (tipsByCategory.Any())
                        {
                            foreach (var tip in tipsByCategory)
                            {
                                Console.WriteLine($"\nID: {tip.Id}");
                                Console.WriteLine($"Title: {tip.Title}");
                                Console.WriteLine($"Description: {tip.Description}");
                                Console.WriteLine($"Category: {tip.Category}");
                                Console.WriteLine($"Created Date: {tip.CreatedDate:dd/MM/yyyy HH:mm:ss}");
                                Console.WriteLine($"Upvotes: {tip.Upvotes}");
                                Console.WriteLine($"Downvotes: {tip.Downvotes}");
                            }
                        }
                        else
                        {
                            Console.WriteLine("No tips found in this category.");
                        }
                        break;
                    case 5:
                        Console.Write("Enter number of top rated tips to display: ");
                        int topTipCount = int.Parse(Console.ReadLine());
                        var topRatedTips = tipManager.GetTopRatedTips(topTipCount);
                        if (topRatedTips.Any())
                        {
                            foreach (var tip in topRatedTips)
                            {
                                Console.WriteLine($"\nID: {tip.Id}");
                                Console.WriteLine($"Title: {tip.Title}");
                                Console.WriteLine($"Description: {tip.Description}");
                                Console.WriteLine($"Category: {tip.Category}");
                                Console.WriteLine($"Created Date: {tip.CreatedDate:dd/MM/yyyy HH:mm:ss}");
                                Console.WriteLine($"Upvotes: {tip.Upvotes}");
                                Console.WriteLine($"Downvotes: {tip.Downvotes}");
                            }
                        }
                        else
                        {
                            Console.WriteLine("No tips found.");
                        }
                        break;
                    case 6:
                        Console.Write("Enter tip ID to upvote: ");
                        int upvoteId = int.Parse(Console.ReadLine());
                        tipManager.UpvoteTip(upvoteId);
                        Console.WriteLine("Tip upvoted successfully.");
                        break;
                    case 7:
                        Console.Write("Enter tip ID to downvote: ");
                        int downvoteId = int.Parse(Console.ReadLine());
                        tipManager.DownvoteTip(downvoteId);
                        Console.WriteLine("Tip downvoted successfully.");
                        break;
                    case 8:
                        Console.WriteLine("Exiting program.");
                        return;
                    default:
                        Console.WriteLine("Invalid choice. Please try again.");
                        break;
                }
            }
        }
    }
}
