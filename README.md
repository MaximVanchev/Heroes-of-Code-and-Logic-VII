# Heroes-of-Code-and-Logic-VII
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace _3._Heroes_of_Code_and_Logic_VII
{
    class Program
    {
        static void Main(string[] args)
        {
            Dictionary<string, Heroes> heroes = new Dictionary<string, Heroes>();
            int heroesCount = int.Parse(Console.ReadLine());
            for (int i = 0; i < heroesCount; i++)
            {
                List<string> hero = Console.ReadLine().Split(" ", StringSplitOptions.RemoveEmptyEntries).ToList();
                string heroName = hero[0];
                double heroHelth = double.Parse(hero[1]);
                double heroMana = double.Parse(hero[2]);
                if (heroHelth > 100)
                {
                    heroHelth = 100;
                }
                if (heroMana > 200)
                {
                    heroMana = 200;
                }
                heroes.Add(heroName, new Heroes(heroHelth, heroMana));
            }
            List<string> command = Console.ReadLine().Split(" - ",StringSplitOptions.RemoveEmptyEntries).ToList();
            while (command[0] != "End")
            {
                if (command[0] == "CastSpell")
                {
                    if (heroes[command[1]].mana >= double.Parse(command[2]))
                    {
                        heroes[command[1]].mana = heroes[command[1]].mana - double.Parse(command[2]);
                        Console.WriteLine($"{command[1]} has successfully cast {command[3]} and now has {heroes[command[1]].mana} MP!");
                    }
                    else
                    {
                        Console.WriteLine($"{command[1]} does not have enough MP to cast {command[3]}!");
                    }
                }
                else if (command[0] == "TakeDamage")
                {
                    if (heroes[command[1]].health > double.Parse(command[2]))
                    {
                        heroes[command[1]].health = heroes[command[1]].health - double.Parse(command[2]);
                        Console.WriteLine($"{command[1]} was hit for {command[2]} HP by {command[3]} and now has {heroes[command[1]].health} HP left!");
                    }
                    else
                    {
                        Console.WriteLine($"{command[1]} has been killed by {command[3]}!");
                        heroes.Remove(command[1]);
                    }
                }
                else if (command[0] == "Recharge")
                {
                    if (heroes[command[1]].mana + double.Parse(command[2]) > 200)
                    {
                        double currentInt = 200 - heroes[command[1]].mana;
                        Console.WriteLine($"{command[1]} recharged for {currentInt} MP!");
                        heroes[command[1]].mana = 200;
                    }
                    else
                    {
                        Console.WriteLine($"{command[1]} recharged for {command[2]} MP!");
                        heroes[command[1]].mana = heroes[command[1]].mana + double.Parse(command[2]);
                    }
                }
                else if (command[0] == "Heal")
                {
                    if (heroes[command[1]].health + double.Parse(command[2]) > 100)
                    {
                        double currentInt = 100 - heroes[command[1]].health;
                        Console.WriteLine($"{command[1]} healed for {currentInt} HP!");
                        heroes[command[1]].health = 100;
                    }
                    else
                    {
                        Console.WriteLine($"{command[1]} healed for {command[2]} HP!");
                        heroes[command[1]].health = heroes[command[1]].health + double.Parse(command[2]);
                    }
                }
                command = Console.ReadLine().Split(" - ", StringSplitOptions.RemoveEmptyEntries).ToList();
            }
            foreach (var item in heroes.OrderByDescending(x => x.Value.health).ThenBy(x => x))
            {
                Console.WriteLine(item.Key);
                Console.WriteLine(item.Value);
            }
        }
    }
    public class Heroes
    {
        public double health { get; set; }
        public double mana { get; set; }
        public Heroes(double HP,double MP)
        {
            health = HP;
            mana = MP;
        }
        public override string ToString()
        {
            StringBuilder sb = new StringBuilder();
            sb.AppendLine($"HP: {health}");
            sb.Append($"MP: {mana}");
            return sb.ToString();
        }
    }
}
