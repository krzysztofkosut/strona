require "open-uri"
require "json"
require "cgi/html"

puts("Prosze czekac.")

num = File.open("nr.txt","r")
file = File.open("linki.txt", "w")

#tworzenie pliku z linkami do obrazkow
  while not (num.eof?) do
    s = "https://xkcd.com/"
    s[17] = num.readline.to_i.to_s
    s[s.length] = "/info.0.json"
    URI.open(s) {|f|
      dane = (f.read)
      dane_hash = JSON.parse(dane)
      file.write(dane_hash["img"])
      file.puts("")
    }
  end

file.close
num.close

#tworzenie html
f = File.open("strona.html","w")
f.write("<html><center>")
file = File.open("linki.txt","r")
while not (file.eof?) do
  a = file.readline
  f.write('<img src="')
  f.write(a)
  f.write('"width="50%"><br>')
end
f.write("<html>")
f.close

print("Gotowe. Prosze wcisnac enter.")
gets
