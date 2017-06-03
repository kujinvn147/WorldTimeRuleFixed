package com.wtr.fixed;

import org.bukkit.World;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.command.ConsoleCommandSender;
import org.bukkit.entity.Player;
import org.bukkit.plugin.Plugin;
import org.bukkit.plugin.java.JavaPlugin;

import net.md_5.bungee.api.ChatColor;

public class Main extends JavaPlugin {

	String cslprefix = "[WorldTimeRule] ";

	public boolean isnight = false;
	
	public static Plugin plugin;

	public static Plugin getPlugin() {
		return plugin;
	}

	public void loadingConfiguration() {

		String world = "world";
		plugin.getConfig().addDefault(world, "world");

		String prefix1 = "prefix.day";
		plugin.getConfig().addDefault(prefix1, "&f[&eBAN NGÀY&f] &r> ");
		String prefix2 = "prefix.night";
		plugin.getConfig().addDefault(prefix2, "&f[&dBAN ĐÊM&f] &r> ");
		String prefix3 = "prefix.warning";
		plugin.getConfig().addDefault(prefix3, "&7[&cCẢNH BÁO&7] &r> ");

		String msg1 = "msg.day";
		plugin.getConfig().addDefault(msg1, "&bChết không mất đồ");
		String msg2 = "msg.night";
		plugin.getConfig().addDefault(msg2, "&cChết có mất đồ!");
		String msg3 = "msg.warning";
		plugin.getConfig().addDefault(msg3, "&cCòn &f%tick &cgiây nữa sẽ chuyển qua non-keep");

		String title = "title.day";
		plugin.getConfig().addDefault(title, "&e&lBAN NGÀY");
		String title2 = "title.night";
		plugin.getConfig().addDefault(title2, "&d&lBAN ĐÊM");

		String subtitle = "subtitle.day";
		plugin.getConfig().addDefault(subtitle, "&bChết không mất đồ, hãy bắt đầu chuyến đi");
		String subtitle2 = "subtitle.night";
		plugin.getConfig().addDefault(subtitle2, "&eChết có mất đồ, hãy cẩn thận!");

		getConfig().options().copyDefaults(true);
		saveDefaultConfig();
	}

	@SuppressWarnings("deprecation")
	@Override
	public void onEnable() {
		ConsoleCommandSender console = getServer().getConsoleSender();
		console.sendMessage(cslprefix + "Fixed by ExplorerVN");
		console.sendMessage(cslprefix + "Source: Trong Tuan");

		plugin = this;
		loadingConfiguration();

		World w = getServer().getWorld(getConfig().getString("world"));

		getServer().getScheduler().scheduleAsyncRepeatingTask(this, new Runnable() {
			public void run() {
				if (w == null) {
					return;
				}
				if ((Main.this.isnight) && (w.getTime() >= 0L) && (w.getTime() < 13700L)) {
					Main.this.isnight = false;
					w.setGameRuleValue("keepInventory", "true");
					for (Player p : w.getPlayers()) {
						Titles.sendTitle(p, getConfig().getString("title.day").replace("&", "§"),
								getConfig().getString("subtitle.day").replace("&", "§"), 20, 40, 20);
						p.sendMessage(getConfig().getString("prefix.day").replace("&", "§")
								+ getConfig().getString("msg.day").replace("&", "§"));
					}
				} else if ((!Main.this.isnight) && (w.getTime() >= 13700L)) {
					Main.this.isnight = true;
					w.setGameRuleValue("keepInventory", "false");
					for (Player p : w.getPlayers()) {
						Titles.sendTitle(p, getConfig().getString("title.night").replace("&", "§"),
								getConfig().getString("subtitle.night").replace("&", "§"), 20, 40, 20);
						p.sendMessage(getConfig().getString("prefix.night").replace("&", "§")
								+ getConfig().getString("msg.night").replace("&", "§"));
					}
				} else if ((!Main.this.isnight) && (w.getTime() >= 13500L)) {
					int giay = (int) ((13700L - w.getTime()) / 20L) + 1;
					if (giay == 10) {
						for (Player p : w.getPlayers()) {
							p.sendMessage(getConfig().getString("prefix.warning").replace("&", "§") + getConfig()
									.getString("msg.warning").replace("&", "§").replace("%tick", String.valueOf(giay)));
						}
					}
				}
			}
		}, 20, 20);
	}

	@Override
	public void onDisable() {
	}

	@Override
	public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args) {
		if (sender instanceof Player) {
			Player p = (Player) sender;
			if (cmd.getName().equalsIgnoreCase("wtrreload")) {
				if (args.length == 0) {
					plugin.reloadConfig();
					p.sendMessage(ChatColor.GREEN + "Reload complete.");
				} else if (args.length >= 1) {
					p.sendMessage(ChatColor.GRAY + "/wtrreload");
				}
			}
		} else {
			sender.sendMessage(cslprefix + ChatColor.RED + "Lenh chi co the su dung trong tro choi!");
		}
		return true;
	}

}
