#!/usr/bin/env ruby

COMMAND_DIR = File.expand_path(File.dirname(__FILE__))
BASENAME = File.basename(__FILE__)
PREFIX = "#{BASENAME}-"


class MagicCli
    attr_reader :commands

    def initialize()
        @commands = Dir.glob(File.join(File.dirname(__FILE__), "#{PREFIX}*"))
    end

    def list_commands
        subcommands = []
        @commands.each do |filename|
            description = self.get_description(filename)
            parameters = self.get_parameters(filename).join(" ")
            # Remove the `BASENAME-` PREFIX from the filename to get the name of the subcommand
            subcommand = File.basename(filename)[(BASENAME.size + 1)..-1]
            subcommands.push({
                :name => [subcommand, parameters].join(" "),
                :description => description
            })
        end
        max_subcommand_name_length = subcommands.map{ |subcommand| subcommand[:name].length }.max

        subcommands
           .sort_by { |subcommand| subcommand[:name] }
           .each do |subcommand|
               puts "   %-#{max_subcommand_name_length}s   %s" % [subcommand[:name], subcommand[:description]]
           end
    end

    def get_description(filename)
        # Optionally, subcommands can put a description on the second line of the file
        lines = File.readlines(filename)
        description = if lines[1] && lines[1].strip.start_with?('#')
            lines[1].strip.gsub(/^#\s*/, '')
        else
            nil
        end
        return description
    end

    def get_parameters(filename)
      # Optionally, retrieve parameters that are required
      lines = File.readlines(filename)
      header = lines.take_while{|l| l.strip.start_with?('#')}
      parameters = header.map do |line|
        if line.strip.start_with?('# @param')
          line = line.strip.sub(/^# @param\s*/, '').sub(/-.*$/, '').strip
          line = "<param>" if line.empty?
          line
        else
          nil
        end
      end.compact
      return parameters
    end

    def execute(command, args)
        executable = File.join(COMMAND_DIR, PREFIX + command)
        unless File.exist?(executable)
            puts "I don't know how to #{command}. :("
            abort
        end

        exec executable, *ARGV
    end
end

command = ARGV.shift
cli = MagicCli.new
case command
    when nil, '--help', '-h'
        puts "usage: #{BASENAME} <command> [<args>]"
        puts ''

        if cli.commands.empty?
            puts 'Hrm, there are no commands for me to run.'
            puts "I can run any executables in #{COMMAND_DIR} which have filenames that start with `#{PREFIX}`."
            abort
        else
            puts 'Commands:'
            cli.list_commands
        end
    when '--list', '-l'
        cli.list_commands
    else
        cli.execute(command, ARGV)
end
