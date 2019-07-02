word_count_ultimate_edition.exs

```elixir
filename =
  IO.gets("File to count the words from (h for help):\n")
  |> String.trim()

if filename == "h" do
  IO.puts("""
  Usage: [filename] -[flags]
  Flags
  -l     displays line count
  -c     displays character count
  -w     displays word count (default)
  Multiple flags may be used. Example usage to display line and character count:
  somefile.txt -lc
  """)
else
  parts = String.split(filename, "-")

  filename =
    List.first(parts)
    # to remove spaces in start and end
    |> String.trim()

  # return items in the list by index
  flags =
    case Enum.at(parts, 1) do
      # set only "w" flag if none were set
      nil -> ["w"]
      # String.split on "" return caracters.
      chars -> String.split(chars, "") |> Enum.filter(fn x -> x != "" end)
    end

  body = File.read!(filename)

  # some systems use \r for new line and another systems use \r\n for new line -> here we use all with OR operator to catch any of them
  lines = String.split(body, ~r{(\r\n|\n|\r)})

  words =
    String.split(body, ~r{(\\n|[^\w'])+})
    |> Enum.filter(fn x -> x != "" end)

  chars = String.split(body, "") |> Enum.filter(fn x -> x != "" end)

  # will run for each item in the list
  Enum.each(flags, fn flag ->
    case flag do
      "l" -> IO.puts("Lines: #{Enum.count(lines)}")
      "w" -> IO.puts("Words: #{Enum.count(words)}")
      "c" -> IO.puts("Chars: #{Enum.count(chars)}")
      _ -> nil
    end
  end)
end

```


