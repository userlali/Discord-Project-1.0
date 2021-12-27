import discord
import random
from discord.ext import commands

token=''


client = commands.Bot (command_prefix = ".")

@client.event

async def on_ready():
    print('Bot is ready.')
    await client.change_presence(activity=discord.Game(name=".help"))

@client.command()
@commands.has_permissions(administrator=True)
async def activity(ctx,*,activity):
    await client.change_presence(activity=discord.Game(name=activity))
    await ctx.send(f"Bot activity has been changed to '{activity}'!")
#.activity ....


client.remove_command("help")

@client.command()
@commands.has_permissions(kick_members=True)
async def kick(ctx, member : discord.Member,*, reason=None):
    await member.kick(reason=reason)
    await ctx.send(f'{member} has been kicked!')
#.kick @...

@client.command()
@commands.has_permissions(ban_members=True)
async def ban(ctx, member : discord.Member, *, reason=None):
    await member.ban(reason=reason)
    await ctx.send(f'{member} has been banned!')
#.ban @...


@client.command()
async def userinfo(ctx):
    user = ctx.author

    embed = discord.Embed(title = "USER INFO", description = f'Here is all of the info we collected about {user}', color = user.color)
    embed.set_thumbnail(url=user.avatar_url)
    embed.add_field(name='NAME', value=user.name, inline=True)
    embed.add_field(name='NICKNAME', value=user.nick, inline=True)
    embed.add_field(name='ID', value=user.id, inline=True)
    embed.add_field(name='STATUS', value=user.status, inline=True)
    embed.add_field(name='TOP ROLE', value=user.top_role.name, inline=True)
    await ctx.send(embed=embed)
#.userinfo



@client.command()
async def help(ctx, commandSent=None):
    if commandSent != None:
        for command in client.commands:
            if commandSent.lower() == command.name:
                paramString=''
                for param in command.clean_params:
                    paramString += param
                embed = discord.Embed(title=f"HELP - {command.name}")
                embed.add_field(name='paramaters', value = paramString)
                await ctx.message.delete()
                await ctx.author.send(embed=embed)


    else:
        embed=discord.Embed(title="LIST OF COMMANDS")
        embed.add_field(name=".ban", value="Ban a member", inline=True)
        embed.add_field(name=".kick", value="Kick a member", inline=True)
        embed.add_field(name=".userinfo", value="See your profile information!", inline=True)
        embed.add_field(name=".activity", value="This allows you to change the bots 'activity'", inline=True)

    await ctx.message.delete()
    await ctx.author.send(embed=embed)






client.run(token)
